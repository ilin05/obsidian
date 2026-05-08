# §1 Introduction — Draft v1

**Paper:** CXL-Vector (VLDB submission)
**Target length:** ~1.5 pages PVLDB single-column (≈ 1,050 words)
**Narrative arc:** Promise → Collapse → Insight → System → Contributions
**Status:** 2026-04-11

---

## 1. Introduction

### ¶1 — The capacity wall of modern vector search

Large language models, semantic search, and retrieval-augmented generation have made approximate nearest-neighbor search (ANNS) a foundational workload of the modern data stack. Production deployments now routinely maintain indexes of 10 billion or more vectors at 128–768 dimensions: a 10 B × 768d float32 corpus alone is over 30 TB of raw embedding data, before any graph structure, quantization codebook, or operating-system overhead. Hierarchical graph indexes such as HNSW [Malkov & Yashunin 2018] and Vamana [Subramanya et al. 2019] deliver sub-millisecond query latency at Recall@10 > 95% *when the entire index fits in local DRAM* — a condition that billion-scale workloads have now decisively outgrown. Commodity servers cap at 1–2 TB of DDR5, and that budget is already contested by model weights, feature caches, and co-resident services. The ANNS community has responded with SSD-centric designs (DiskANN [NeurIPS'19], SPANN [NeurIPS'21]), which trade per-query latency for capacity by structuring the index around large sequential I/Os; a single NVMe seek at 100–200 µs, however, exceeds the end-to-end latency budget of interactive retrieval by two orders of magnitude. The capacity crisis and the latency SLA have become mutually exclusive.

### ¶2 — CXL as the missing memory tier

**Compute Express Link (CXL)** breaks this binary. CXL attaches DRAM-backed memory to a host via PCIe 5.0 as cache-coherent, byte-addressable memory. True CXL hardware, as characterized on Intel Sapphire Rapids systems [MICRO'23], exposes idle-load latencies of 29–41 ns — only 3–4× slower than local DDR5 (~76 ns), and five orders of magnitude faster than NVMe SSD. Per-device capacities of 256 GB to 4 TB at roughly 30–50% of DRAM cost per GB position CXL precisely in the gap that the vector-search community has been struggling to fill: *a tier that is fast enough for pointer-chasing graph traversal and large enough to hold billion-scale embedding tables*. On paper, CXL is the perfect capacity tier for ANNS serving. In practice, a direct port of HNSW to CXL collapses long before it can exploit any of this opportunity.

### ¶3 — The collapse: naive CXL-HNSW does not scale

To make this concrete, we ran a microbenchmark on an Intel Xeon Gold 5520+ host (56 hardware threads, 1 CXL Type-3 device exposed as NUMA node 3). We pointed hnswlib's allocator at node 3 via `mbind(bind:3)`, built an HNSW index for SIFT-10M, and swept the thread count from 1 to 56 (Figure 2). The result is stark: **throughput plateaus between 4 and 8 threads and *decreases* thereafter** — at 56 threads, QPS is lower than at 8. The same experiment with raw vectors in local DRAM scales near-linearly. The bottleneck is not compute and not graph complexity. It is PCIe bandwidth — saturated by a workload that was never designed to treat bandwidth as a scarce resource.

### ¶4 — Three compounding failure modes

Three structural properties of vanilla HNSW compound this collapse, each attacking a different physical resource:

1. **Unconstrained remote raw-vector access.** hnswlib's node-centric memory layout interleaves edge lists, the raw float vector, and per-node metadata in a single block. Every graph hop — even one that only needs to compute an approximate distance for routing — unconditionally drags 512+ bytes of raw vector across PCIe. On DRAM this over-fetch is harmless; on CXL it is the dominant traffic source. Measured PCIe utilization at t=16 already exceeds 80% of a single CXL channel's 25–38 GB/s ceiling.

2. **Single-query execution blocking.** Each worker thread drives one query end-to-end, issuing a blocking load at every graph hop. When that load misses in LLC and crosses PCIe to CXL, the thread stalls with no overlap. At 29 ns per CXL access and 200+ hops per SIFT-10M query, each thread accumulates multiple microseconds of irreducible serial latency per query — latency that more threads cannot hide, because more threads simply produce more concurrent stalls.

3. **Thread count as unmanaged bandwidth consumer.** HNSW serving systems treat thread count as a monotonically beneficial knob. Under CXL, this is actively harmful: past the point of PCIe saturation, additional threads inflate queuing delay at the CXL controller, driving P99 latency from 75 µs at t=1 to 3,256 µs at t=56 in our ablation measurements — a 43× tail-latency regression for zero throughput gain.

### ¶5 — What CXL-ANNS solved, and what it did not

Recent work on CXL-aware ANNS — most prominently **CXL-ANNS** [ACM ToS 2024] — addresses these failures through hardware-software co-design. CXL-ANNS builds custom CXL Type-3 endpoints with on-device Domain-Specific Accelerators (DSAs) that push distance computation down to the memory side; only scalar distance values ever cross PCIe, eliminating the bandwidth bottleneck at its source. This is an elegant and powerful result — but it depends on FPGA-based or ASIC endpoints that do not yet exist as deployable commodity products. CXL devices shipping today are Type-3 memory expanders without compute capability. The question CXL-ANNS answers is *"what is possible with future CXL silicon?"*; the question we address is **"what is possible on the CXL silicon that exists today, in pure software, with PCIe bandwidth as a hard ceiling?"** These are complementary results, not competing ones — but the latter is the one that determines whether current CXL deployments can serve ANNS workloads at all.

### ¶6 — Insight: this is a scheduling problem

This framing forces a reorientation of where the difficulty lies. Data placement — deciding which bytes live on CXL — is the *first* problem, but it is not the hard one: raw vectors are obviously the coldest data. The hard problem is what to do with the PCIe accesses that remain. Our central observation is:

> **Serving HNSW on commodity CXL is primarily a scheduling problem, not a data placement problem.**

Once raw-vector access is confined to a single bounded stage (the final exact rerank), its latency is no longer a correctness constraint — it is a bandwidth budget. And a bandwidth budget can be *scheduled*: by interleaving concurrent queries within a single worker thread, the time one query spends waiting on a CXL fetch can be spent computing SQ8 distances for another query whose data is already hot. This is not a new idea in systems: it is the same insight that underlies **morsel-driven parallelism** [Leis et al., SIGMOD'14] in modern analytical query engines, where cache misses on one tuple are hidden by computation on the next. Our contribution is to recognize that HNSW's 3-stage search structure — upper-layer routing, level-0 expansion, exact rerank — is an almost perfect fit for this execution model, once the memory hierarchy is laid out correctly.

### ¶7 — CXL-Vector

We introduce **CXL-Vector**, a pure-software HNSW serving runtime whose entire design is shaped by the PCIe-bandwidth constraint. CXL-Vector addresses the three failure modes of §¶4 with three co-designed components, each targeting a different physical resource:

- *Against unconstrained remote access (¶4.1),* CXL-Vector performs a **build→serve layout conversion**: an offline `convertBuildExactToCXL` pass flattens the node-centric build-time layout into a segmented serving layout where SQ8 codes, CSR-style level-0 edges, and upper links live in DRAM, and only the `vec_segment_` of raw floats resides on CXL. Graph routing never touches CXL.

- *Against single-query blocking (¶4.2),* CXL-Vector drives the hot loop entirely on DRAM-resident SQ8 codes via an **AVX-512 VNNI coarse-to-exact search path**. Distance computation in the level-0 expansion becomes an 8-bit integer dot product that fits in L2 cache; CXL access is reserved for a bounded exact-rerank stage whose size (16–64 candidates per query) is bounded by an adaptive budget.

- *Against unmanaged bandwidth consumption (¶4.3),* CXL-Vector executes batches through a **morsel-driven scheduler** with an Issue/Consume pipeline per-query state. Worker threads interleave the neighbor-prefetch phase of one query with the distance-compute phase of another, transforming concurrent queries from a source of PCIe contention into a mechanism for hiding CXL latency. Thread count becomes a governed bandwidth budget, not a free parameter.

Taken together, these three components make PCIe bandwidth an explicit scheduled resource throughout the serving pipeline.

### ¶8 — Contributions

This paper makes the following contributions:

- **Problem characterization.** We provide the first systematic empirical decomposition of why HNSW serving collapses on commodity CXL hardware, identifying three compounding failure modes (unconstrained remote access, single-query blocking, unmanaged bandwidth) and grounding them in a thread-scaling microbenchmark and an ablation study (§3).

- **Layered data placement for CXL serving.** A build→serve two-stage architecture that cleanly separates a classical DRAM-optimal build layout from a read-only CXL-optimized serving layout, with CSR-style level-0 adjacency and arena-allocated upper links. All routing-critical structures remain in DRAM while raw float vectors are offloaded to CXL (§4).

- **Coarse-to-exact search path with bounded CXL access.** A distance-kernel dispatcher that routes upper-layer and level-0 distances through AVX-512 VNNI SQ8 kernels, together with a controlled exact-rerank budget and patience-based early termination. Together these mechanisms confine CXL access to a bounded, amortizable stage (§5).

- **Morsel-driven batch execution with Issue/Consume pipelining.** A QueryState-based execution model and morsel-driven scheduler that interleave concurrent queries to hide CXL latency, treating PCIe bandwidth as a first-class scheduled resource and achieving 59% linear-scaling efficiency to 56 threads (§6).

- **Evaluation and physical NUMA verification.** On 8 standard ANNS benchmarks (1 M–500 M vectors), CXL-Vector achieves 2.71×–6.37× higher QPS than HNSWLib, 3.09×–6.78× lower per-query latency than DiskANN-vamana, and Recall@10 within 0.38 pp of every baseline. Kernel-level `/proc/<pid>/numa_maps` sampling confirms that the 488 MB raw-vector segment resides entirely on the CXL node (policy=bind:3, 125,000/125,000 pages on node 3), converting the placement claim from a software-level assumption into a physically-observable property (§8).

### ¶9 — Paper organization

§2 reviews HNSW serving, CXL memory, scalar quantization, and morsel-driven execution. §3 presents the microbenchmark evidence for the three failure modes that motivate CXL-Vector. §4–§6 describe the three technical components in turn. §7 summarizes the implementation. §8 presents the evaluation. §9 discusses related work. §10 concludes.

---

## Writing notes for this draft

### Word count
- ¶1 Capacity wall: ~170 words
- ¶2 CXL opportunity: ~110 words
- ¶3 Microbench setup: ~120 words
- ¶4 Three failure modes: ~220 words
- ¶5 CXL-ANNS positioning: ~170 words
- ¶6 Insight: ~180 words
- ¶7 System overview: ~230 words
- ¶8 Contributions: ~280 words (bullets)
- ¶9 Organization: ~40 words
- **Total: ~1,520 words** → about 15–20% long. Trim targets in the next pass: ¶1 (can lose ~30), ¶4 (¶4.3 has a redundant "43×" that can move to §3), ¶7 (~30), ¶8 (~30).

### Sentence-level polish items flagged for v2
- ¶1 "foundational workload of the modern data stack" — slightly marketing. Consider "a core primitive for LLM retrieval pipelines."
- ¶3 "The result is stark" — might read as hype; consider "Throughput plateaus between 4 and 8 threads and then decreases…"
- ¶5 "elegant and powerful result" — slightly sycophantic; consider "a compelling proof point" or just "effective".
- ¶6 The morsel-driven parallelism citation is load-bearing; make sure §9 Related Work returns to it.
- ¶8 Each contribution bullet currently leads with a noun phrase ("Layered data placement", "Coarse-to-exact search"); keep parallel structure.

### Reviewer-facing anticipations (already addressed inline)
- ✅ "Why not compare to CXL-ANNS?" → ¶5 explicitly frames the two as complementary with a different research question
- ✅ "Is the bottleneck really PCIe?" → ¶3 microbench + ¶4.1 explicit bandwidth number (80% of channel at t=16)
- ✅ "Why morsel-driven and not something simpler?" → ¶6 ties back to SIGMOD'14, a venue reviewers respect
- ✅ "How do you prove placement actually happened?" → ¶8 final bullet cites numa_maps
- ⚠️ "Why HNSW and not Vamana/NSG?" → not addressed in §1; needs a line in §2 or §9
- ⚠️ "What about CPU-only DRAM optimization (FAISS-HNSW-SQ8)?" → not in §1; should land in §3 motivation microbench set

### Figures referenced from §1
- **Figure 1** (¶2): memory tier landscape (DRAM / CXL / NVM / SSD on latency × capacity × $/GB axes). Still to be drawn.
- **Figure 2** (¶3): naive CXL-HNSW thread-scaling collapse. Already exists via `comp.py` + `thread_scaling.csv`.

### Open items for v2
- [ ] Trim total to ~1,100 words to hit the 1.5-page PVLDB budget
- [ ] Decide whether ¶5 (CXL-ANNS positioning) stays in §1 or moves to §9 Related Work. My preference: keep in §1 because it defines the paper's research question and every reviewer will ask this question in the first 30 seconds.
- [ ] Add one sentence in ¶3 acknowledging that we also tested FAISS-HNSW-SQ8 and it has the same problem (once the FAISS experiment is run)
- [ ] Hyperlink the "three failure modes" in ¶4 to the three contributions in ¶8 explicitly (e.g., "(addressed by §4)", "(addressed by §5)", "(addressed by §6)") to make the problem→solution mapping impossible to miss
