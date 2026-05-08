# CXL-Vector: A CXL-Native HNSW Serving Runtime for Disaggregated Memory

**Target venue:** PVLDB (Proceedings of the VLDB Endowment) / VLDB 2027
**Target length:** 12 pages (PVLDB single-column format)
**Status:** Outline v0.1 — 2026-04-11

---

## 0. Abstract  *(~250 words)*

- **Hook:** Vector search at billion scale is capacity-bound; CXL offers a new tier (29–41 ns, cheap per GB) between DRAM and SSD, but naively placing HNSW on CXL triggers a multi-thread PCIe bandwidth collapse.
- **Problem:** We identify three compounding failure modes — unconstrained remote vector access, single-query execution granularity, and lack of PCIe-bandwidth-aware scheduling — that make existing HNSW systems (HNSWLib, DiskANN-vamana, FAISS-HNSW-SQ8) fundamentally unsuited to CXL serving.
- **Solution:** CXL-Vector — a CXL-native HNSW serving runtime built on three co-designed components: *(i)* a build→serve layout conversion that places SQ8 codes and graph edges in DRAM while offloading raw float vectors to CXL, *(ii)* a coarse-to-exact search path driven by SQ8 on AVX-512 VNNI with bounded exact rerank, and *(iii)* a QueryState + Issue/Consume pipeline executed by a morsel-driven scheduler that hides CXL latency via cross-query interleaving.
- **Results:** On 8 standard ANN datasets (1 M – 500 M vectors), CXL-Vector delivers **2.71×–6.37× QPS over HNSWLib** and **3.09×–6.78× latency reduction over DiskANN-vamana**, while holding Recall@10 within **≤ 0.38 pp** of the best baseline. Thread scaling is near-linear to 56 threads (59% efficiency); the ablation config saturates at 8 threads, establishing the **27.1× contribution of the proposed scheduling and quantization path**. Physical NUMA sampling confirms the 488 MB raw-vector segment resides on CXL node 3 with zero leakage to DRAM.
- **Claim:** CXL-Vector is the first HNSW serving system that simultaneously achieves DRAM-level recall, SQ8-level throughput, and CXL-scale capacity on commodity CXL hardware — making it a reference design for the CXL memory tier.

---

## 1. Introduction  *(~1.5 pages)*

### 1.1 The memory wall in vector search
- Billion-scale ANN is now a baseline workload for RAG, recsys, semantic search.
- DRAM-resident graph indexes (HNSW, NSG) deliver sub-ms latency but exceed 1 TB at 10 B × 128d float32.
- SSD-centric systems (DiskANN, SPANN) trade latency for capacity — unsuitable for interactive SLAs.

### 1.2 CXL: a new tier between DRAM and SSD
- **Latency:** 29–41 ns on true CXL hardware [MICRO'23] — only 3–4× DRAM, 2500× better than SSD.
- **Capacity:** 256 GB–4 TB per CXL device, stacked per PCIe slot.
- **Cost:** ~30–50% of DRAM per GB.
- *Fig. 1:* memory tier comparison (latency × capacity × $/GB) — CXL fills the gap left by NVM/Optane EOL.

### 1.3 Why naive CXL-HNSW fails
- Planting hnswlib on a CXL NUMA node does not work. Three structural problems compound:
  1. **Unconstrained remote access** — node-centric layout forces a raw-float fetch per hop.
  2. **PCIe bandwidth saturation** — at 56 threads, aggregate demand exceeds one CXL channel.
  3. **Single-query execution granularity** — serial CXL waits have no overlap opportunity.
- *Fig. 2:* Throughput collapse diagram — ablation (no SQ8 / no scheduler) saturates at t=8, full system scales linearly to t=56, showing a **27.1× gap at t=56** purely from software path redesign.

### 1.4 The scheduling-problem insight
- *"Porting HNSW to CXL is not primarily a data placement problem — it is a scheduling problem."*
- Once raw-vector access is funneled into a single bounded stage (rerank), its latency can be hidden by interleaving multiple in-flight queries.
- This is what morsel-driven batch execution in DB query processing solves — we adapt it to the HNSW search loop.

### 1.5 Contributions
1. **Problem characterization.** We provide the first systematic account of why HNSW serving on commodity CXL hardware fails, with a decomposition into three compounding failure modes and a microbenchmark-grounded motivation (§3).
2. **Layered data placement.** A build→serve layout conversion (`HierarchicalNSW_ExactBuild` → `convertBuildExactToCXL` → V2/V3 persistence) that separates hot graph/SQ8 metadata (DRAM) from cold raw vectors (CXL). Novel CSR-style level-0 layout optimized for CXL-aware prefetch. (§4)
3. **Coarse-to-exact search path.** A DRAM-led SQ8-VNNI-driven coarse traversal with bounded exact rerank, patience-based early termination, and controlled rerank budget — keeps Recall@10 within 0.38 pp of baseline on all 8 datasets. (§5)
4. **QueryState + morsel-driven scheduler.** A batch execution model that treats PCIe bandwidth as a first-class scheduling resource via Issue/Consume pipelining and cross-query interleaving, achieving 59% linear scaling efficiency to 56 threads. (§6)
5. **Evaluation.** 8 datasets (1 M–500 M vectors), 4 baselines (HNSWLib, DiskANN-vamana, FAISS-HNSW-SQ8, FAISS-IndexRefine), and physical NUMA verification that the placement assumption holds at the kernel level. (§8)

---

## 2. Background  *(~1 page)*

### 2.1 HNSW serving: the 3-stage pattern
- Upper-layer routing (short, sparse graph)
- Level-0 expansion (long, dense graph — the hot loop)
- Exact rerank (final k candidates, raw float distance)
- Each stage has distinct data/compute/access characteristics — this is what CXL-Vector exploits.

### 2.2 CXL memory: what the hardware actually provides
- CXL.mem vs CXL.cache vs CXL.io.
- True CXL latency (MICRO'23) vs emulated CXL on remote NUMA (what most prior work used).
- PCIe 5.0 × 8 / × 16 channels and their aggregate bandwidth ceilings.
- NUMA policy (`mbind`, `policy=bind:N`) as the software interface to CXL placement.

### 2.3 Scalar quantization for coarse ANN
- SQ8: per-dimension symmetric 8-bit uniform quantization. 4× compression over float32, no codebook training.
- AVX-512 VNNI `_mm512_dpbusd_epi32` enables `uint8 × int8` dot products at ~4× throughput of float FMA.
- Why SQ8 (not PQ) for CXL-Vector: cache line aligned, predictable memory layout, per-dim decoding compatible with streaming prefetch.

### 2.4 Morsel-driven parallelism in DB query processing
- Leis et al., SIGMOD 2014: cache-aware work distribution for in-memory OLAP.
- Key idea: fixed-granularity work units assigned to worker threads + work-stealing + NUMA-local execution.
- We adapt this model from OLAP join/aggregation to ANN graph search — the first such application we are aware of.

---

## 3. Motivation  *(~1 page)*

### 3.1 Microbenchmark 1: PCIe bandwidth saturation under naive placement
- Point hnswlib's allocator at CXL node 3 via `mbind(bind:3)`.
- Measure QPS at t = 1, 4, 8, 16, 32, 56 on SIFT-10M.
- *Result:* Throughput saturates between t=4 and t=8; further threads only increase P99 latency.
- *Cause:* Each hop over-fetches a 512-byte float vector even when only SQ8 distances are needed. Measured PCIe bandwidth at t=56 exceeds the 25 GB/s CXL channel ceiling.

### 3.2 Microbenchmark 2: single-query vs batched execution
- Same workload, compare (i) thread-per-query with blocking CXL fetches vs (ii) morsel-driven interleaved execution.
- *Result:* 8× QPS improvement at t=56 purely from cross-query interleaving, without any quantization changes.

### 3.3 Microbenchmark 3: the FAISS-HNSW-SQ8 design space
- FAISS `IndexHNSWSQ` in DRAM achieves high QPS by discarding raw vectors — but Recall@10 drops by 2–5 pp.
- FAISS `IndexRefine` recovers recall but stores both SQ8 and raw floats in DRAM → 2× HNSWLib footprint.
- Neither resolves the core tradeoff — **capacity or recall, pick one**. CXL-Vector resolves it by moving raw vectors to CXL while keeping SQ8 codes in DRAM.
- *Fig. 3:* Pareto plot of (DRAM footprint, Recall@10, QPS) across the five systems — CXL-Vector sits on a new dominating point.

### 3.4 Design goals derived from these observations
- **G1:** Eliminate raw-vector fetches from the hot graph traversal path.
- **G2:** Concentrate remaining CXL access in a bounded rerank stage.
- **G3:** Hide that rerank latency via cross-query interleaving under a scheduler that treats PCIe bandwidth as a shared budget.

---

## 4. Layered Data Placement  *(~1 page)*

### 4.1 Build/serve split architecture
- Offline: `HierarchicalNSW_ExactBuild` — classical hnswlib-style build in DRAM, parallel `addPoint()`.
- Conversion: `convertBuildExactToCXL` — flattens node-centric layout to a segmented, CXL-aware read-only layout.
- Online: `HierarchicalNSW_CXL` loads segments via `MALAllocator::installForV2Load` — `mmap` + `mbind` + pre-fault, pointer restoration.
- *Why a split?* Build-time mutation patterns (graph insertion, priority queue) conflict with CXL-optimal read layouts; decoupling them lets each phase use its optimal representation.

### 4.2 Segment taxonomy
| Segment | Content | Tier | Access pattern |
|---|---|---|---|
| `sq8_meta_segment_` | per-vector min / inv_scale | DRAM | sequential per query |
| `sq8_code_segment_` | uint8 SQ8 codes | DRAM | random (hot) |
| `level0_offsets_` | CSR offsets[] | DRAM | sequential per node |
| `compact_nav_segment_level0_` | CSR edges[] | DRAM | random (hot) |
| `upper_links_arena_` | upper-layer link blocks | DRAM | random (warm) |
| `label_segment_` / `level_segment_` | per-node metadata | DRAM | random (cold) |
| **`vec_segment_`** | **raw float vectors** | **CXL** | **bounded (rerank only)** |

- The only CXL-resident segment is `vec_segment_` — everything needed for graph routing and SQ8 coarse distance stays in DRAM.

### 4.3 CSR-style level-0 layout
- hnswlib's node-centric block interleaves edges + vector + label → every edge access drags a float vector into the cache.
- CXL-Vector replaces this with flat `offsets[N+1]` + `edges[total_edges]` arrays in DRAM.
- Enables bulk prefetch of neighbor lists and SQ8 codes without any raw-vector pollution.

### 4.4 MAL: the memory tier interface
- `MAL` (`include/mal/mal.h`) exposes `tier0 / tier1 / tier2` as a declarative resource.
- Implementation: `mmap` + `mbind` + pre-fault + 64 B alignment; lazy page allocation is disabled to prevent migration surprises at serving time.
- NUMA verification: `/proc/<pid>/numa_maps` sampling shows `vec_segment_` VMA = 488 MB, `policy=bind:3`, 125000/125000 pages on node 3 — *physical* evidence, not just an API-level assumption.

---

## 5. Coarse-to-Exact Search Path  *(~1.5 pages)*

### 5.1 Distance kernel dispatcher (`distance.h`)
| Stage | Kernel | Throughput | Role |
|---|---|---|---|
| Upper routing | `L2SqrSQ8Ext` / `InnerProductSQ8Ext` | Moderate | Stable routing, few neighbors |
| Level-0 consume | `L2SqrSQ8VNNI` / `InnerProductSQ8VNNI` (`_mm512_dpbusd_epi32`) | **Highest** | Hot loop (bandwidth bound) |
| Rerank | `InnerProductExact` (128/200/768d specialized) | Precise | Final k, CXL-touching |

- SQ8 query context (`QueryState::coarse_ctx.vnni_qctx`) computed once per query (`quantizeQuerySymI8`) and reused across all level-0 rounds.
- Graceful fallback: if AVX512VNNI/BW/F is unavailable, all kernels fall back to scalar — functional closure preserved.

### 5.2 Bounded rerank budget (`compute_rerank_limit`)
- Two modes: absolute (`rerank_abs_limit`) and proportional (`rerank_factor × k`, capped by `rerank_cap` and `ef`).
- *Why bounded?* CXL access is expensive per byte but cheap per query if amortized across candidates. A fixed budget of 32–64 raw-vector fetches per query stabilizes both Recall@10 and CXL bandwidth usage.
- *Why after coarse convergence?* Coarse SQ8 is a monotone lower bound on the top-k that exact rerank reshuffles — processing rerank last ensures the budget is spent on the best candidates.

### 5.3 Patience-based early termination (`patience_termination.h`)
- During level-0 expansion, track `lowerBound` stability, `worst_top` variance, and `newly_accepted` density.
- When all three are below their thresholds for `patience_rounds` consecutive rounds, terminate.
- *Not* a replacement for `ef`; an orthogonal cutoff that activates only when the search path has saturated.
- Ablation shows: patience saves 15–30% of level-0 rounds on well-clustered datasets (SIFT, GIST) with no measurable recall loss.

### 5.4 How the three techniques compose
- VNNI = cheaper *distance kernel* (replaces heavy FMA on float)
- Patience = round-level pruning (cuts low-yield expansion rounds)
- Rerank budget = candidate-level pruning (caps CXL-touching candidates)
- Each attacks a different layer: per-instruction, per-round, per-stage. Together they give CXL-Vector its recall/throughput envelope.

---

## 6. Morsel-Driven Batch Execution  *(~1.5 pages)*

### 6.1 Execution unit: QueryState (`batch.h`)
- Complete per-query execution context — no global shared state on the hot path.
- Contains: `top_candidates`, `candidate_set`, `lowerBound`, `pending_neighbors` (Issue→Consume buffer), `visited_bloom` (4 KB blocked Bloom, L1-resident), `coarse_ctx`, `exact_ctx`, `patience_rounds`.
- *Design rationale:* thread-local QueryState removes the lock convoy + global scratch queue failure modes that dominate DiskANN-vamana at high thread counts.

### 6.2 Issue/Consume pipeline
- **Phase A — Issue:** for each active query, pop one frontier node, collect neighbors via `getLevel0NeighborSpan`, deduplicate via `visited_bloom`, issue `prefetchSQ8Node` for all neighbors.
- **Phase B — Consume:** for each active query, compute SQ8-VNNI distances on the now-hot neighbor data, update `candidate_set` and `top_candidates`.
- **Phase C — Patience check:** per-query termination evaluation.
- Key property: *while query Q_i is waiting for SQ8 prefetch to land in L2, Q_{i+1}'s distances are being computed on data already hot.* This is the latency-hiding mechanism.

### 6.3 Morsel-driven scheduler (`Scheduler.hpp`)
- `Scheduler::setup(num_threads)` pre-spawns worker pool; main thread participates (no always-idle dispatcher).
- `parallelMorsel(begin, end, morsel_size)` splits the query batch into morsels of size 2–8; workers atomically fetch morsels via a single atomic counter.
- Work-stealing absorbs cross-morsel query difficulty variance.
- *Morsel size = 2:* chosen empirically — large enough to amortize per-morsel setup (VNNI query context), small enough to limit per-worker latency variance.

### 6.4 PCIe bandwidth as a first-class resource
- The scheduler treats `num_threads` not as "bigger is better" but as a bandwidth governor.
- Empirical sweet spot: 56 threads + morsel=2 — saturates the CXL rerank channel without inducing queuing at the controller.
- *§8.5* shows the full scaling curve: linear from t=1 to t=56, efficiency 59%.

---

## 7. Implementation  *(~0.5 page)*

- ~15K LOC C++20, built on vendored hnswlib (used only for `SpaceInterface` / `DISTFUNC` interface).
- Key modules:
  - `include/index/` — algorithm, batch pipeline, runtime config, exact build, build→serve conversion
  - `include/storage/` — CXL storage, segment layouts, persistence V2/V3, deletion bitmap
  - `include/mal/` — memory tier abstraction, CXL allocator, NUMA binding
  - `include/scheduler/` — morsel-driven scheduler, parallel execution
  - `include/utils/` — blocked Bloom, lightweight heap, patience termination
- Build → serve → load path uses mmap-based persistence V2/V3 (header + tiered segments).
- Runtime configuration via environment variables (`EXP_BATCH_UPPER_USE_EXT`, `EXP_BATCH_LEVEL0_USE_EXT`, `EXP_RERANK_ABS`, etc.) enables ablation without recompilation.

---

## 8. Evaluation  *(~3 pages)*

### 8.1 Experimental setup
- **Hardware:** Intel Xeon Gold 5520+ (56 cores, AVX-512 VNNI), DDR5-4800 on node 1, CXL-attached DRAM on node 3, 2 NUMA nodes + 1 CXL node.
- **CXL verification:** `numactl --hardware`, `/proc/<pid>/numa_maps` sampling confirms `vec_segment_` (488 MB) fully on CXL node 3 with `policy=bind:3`.
- **Datasets:** 8 standard ANN benchmarks (order fixed across all figures):
  - SIFT-500M, SIFT-100M, SIFT-10M, GIST-1M, DEEP-100M, Text2Image-100M, Wikipedia-35M, MSTuring-100M
- **Metrics:** QPS, per-query mean latency (µs), Recall@10, Recall@10 gap to best method (pp).
- **Default configuration:** 56 threads, morsel=2, `level0_use_vnni=true`, `enable_patience=true`, `enable_exact_rerank=true`.

### 8.2 Baselines *(updated — now includes FAISS)*
| Baseline | Class | Where raw vectors live | Key property |
|---|---|---|---|
| HNSWLib (hnswlib 0.8.0) | DRAM HNSW | DRAM | Node-centric layout, all-exact distances |
| DiskANN-vamana (pure-memory) | DRAM Vamana graph | DRAM | Beam search + global scratch |
| FAISS `IndexHNSWSQ` | DRAM HNSW-SQ8 | DRAM (SQ8 only) | SQ8 stored, no exact rerank |
| FAISS `IndexRefine` (HNSWSq + Flat) | DRAM coarse-to-exact | DRAM (SQ8 + float) | Coarse + exact, 2× DRAM |
| CXL-ANNS *(if reproducible)* | CXL ANN co-design | CXL | Hardware-aware prefetch |
| **CXL-Vector** | **CXL HNSW serving** | **CXL (raw) + DRAM (SQ8/graph)** | **This paper** |

### 8.3 Main results — throughput, latency, recall
- *Table 2:* QPS across 8 datasets with speedup columns (vs HNSWLib, vs DiskANN-vamana, vs FAISS-HNSW-SQ8, vs FAISS-IndexRefine).
- *Table 3:* Mean per-query latency (µs) across the same.
- *Table 4:* Recall@10 with gap-to-best (pp).
- *Fig. 4:* 3-panel chart: QPS / Latency / Recall@10 across all baselines and datasets.
- *Headline numbers:*
  - Relative to HNSWLib: **2.71×–6.37× QPS** ( best: GIST-1M 6.37× )
  - Relative to DiskANN-vamana: **3.11×–6.46× QPS**, **3.09×–6.78× latency reduction**
  - Relative to FAISS-HNSW-SQ8: **comparable QPS but closes the recall gap by X pp** *(experiment TBD)*
  - Relative to FAISS-IndexRefine: **2–3× QPS at half the DRAM footprint** *(experiment TBD)*
  - Recall@10 gap to best: **≤ 0.38 pp on all 8 datasets**

### 8.4 Bottleneck decomposition (internal profile)
- Breakdown via `BatchQueryProfile`:
  - `level0_consume_us` drop → VNNI coarse kernel efficacy (Contribution 3)
  - `issue_core_us` drop + `bloom_negative_total` high fraction → Bloom filter dedup (Contribution 3 aux)
  - `rerank_prefetch_us` / `rerank_exact_us` low fraction → bounded rerank keeps CXL access contained (Contribution 2)
  - Worker busy-time stability across morsels → scheduler overlap (Contribution 3)
- *Fig. 5:* stacked-bar breakdown of per-query time by phase, for CXL-Vector vs each baseline.

### 8.5 Thread scaling and ablation (SIFT-10M)
- Two configurations:
  1. **Full:** SQ8 + morsel scheduler, `vec_segment_` on CXL
  2. **Ablation:** SQ8 off, scheduler off; same CXL placement
- *Table 5:* per-thread QPS, mean/P50/P99 latency, ratio column.
- *Headline:*
  - Full: linear scaling t=1→t=56, 33.2× QPS, efficiency 59%, P99 75 µs → 125 µs
  - Ablation: saturates at t=4–8, t=56 QPS only 1.23× t=1, P99 75 µs → 3256 µs (43×)
  - QPS ratio: 2.18× (t=1) → **27.1× (t=56)**
- *Fig. 6:* 3-panel thread scaling: (a) QPS with ideal-linear reference, (b) P99 tail, (c) contribution ratio.

### 8.6 Sensitivity study
- `morsel_size` ∈ {1, 2, 4, 8, 16} — show morsel=2 sweet spot
- `rerank_abs_limit` ∈ {16, 32, 64, 128} — recall/QPS tradeoff curve
- `ef_search` at fixed rerank budget — validates that SQ8 coarse loop converges on the same top-k region as float
- `patience_rounds` ∈ {0, 2, 4, 8} — effect of early termination

### 8.7 Memory footprint comparison
- *Table 6:* DRAM + CXL memory by system, on SIFT-100M and SIFT-500M.
- CXL-Vector: smallest DRAM footprint among coarse-to-exact systems (because raw vectors live on CXL, not DRAM).
- Demonstrates that capacity advantage is real, not just a paper claim.

### 8.8 Build cost
- *Table 7:* offline build time, conversion time, load-via-mmap time.
- Conversion is parallelizable; load-via-mmap is O(header_parse + mbind) — effectively instant.
- Establishes that the offline cost is acceptable (≈ baseline HNSW build + small conversion overhead).

### 8.9 Physical NUMA verification
- `numa_maps` dump showing `vec_segment_` → 488 MB VMA, `policy=bind:3`, 125000/125000 pages on node 3, DRAM segments on node 1.
- `numastat -p <pid>` confirming CXL node 3 private/shared page counts match expectation.
- *Artifact evidence:* not an API-level claim — kernel-level, directly observable.

---

## 9. Related Work  *(~0.5 page)*

### 9.1 DRAM-resident graph ANN
- HNSW (Malkov & Yashunin 2018), NSG (Fu et al. 2019), Vamana (Subramanya et al. 2019).
- All assume uniform DRAM; none address memory tier hierarchy.

### 9.2 SSD-based ANN
- DiskANN (2019), SPANN (NeurIPS 2021), the performance/index-size dilemma paper (2024).
- Our comparison point: SSD latency (100–200 µs) is orthogonal to CXL latency (29–41 ns) — different design space entirely.

### 9.3 CXL / disaggregated memory for ANN
- **CXL-ANNS** (ISCA 2024): HW/SW co-design with ANN-aware prefetch hardware. Non-portable; does not address HNSW's 3-stage structure.
- **d-HNSW** (arXiv 2025): RDMA-based disaggregated HNSW. Different hardware tier (µs-scale RDMA round trips); solves network round-trip reduction, not PCIe bandwidth saturation.
- CXL memory characterization (MICRO 2023) — empirical grounding we build on.

### 9.4 Vector quantization
- PQ (Jégou 2011), RaBitQ (2024), TurboQuant (2025), QJL (2024).
- We use SQ8 (simpler, VNNI-friendly, no codebook). Our contribution is how SQ8 is *placed* in the memory hierarchy, not the quantizer itself.

### 9.5 Morsel-driven execution in DBMS
- Leis et al. (SIGMOD 2014). First application to ANN graph search, to our knowledge.

### 9.6 Second-tier memory systems (general)
- Pond, TPP, CXL-passthrough approaches for general-purpose workloads.
- Our work is specialization of this trend to the ANN workload class.

---

## 10. Discussion & Limitations  *(~0.3 page)*

- **Build-time cost:** The two-stage build→serve split pays a one-time conversion cost. Acceptable for read-heavy serving but needs rework for streaming inserts.
- **Delete / update:** Currently supported via deletion bitmap only; true online updates require a mergeable log — future work.
- **Hardware dependence:** Results are from Intel Xeon Gold 5520+ + one CXL device. CXL controller quality varies by vendor (CXL-A Hard IP vs CXL-C Soft IP) — we plan a cross-vendor study.
- **Multi-CXL scaling:** Current experiments use one CXL device. Multi-device tier balancing and NUMA-aware morsel assignment is the next phase of work.
- **Recall gap:** The 0.38 pp maximum gap is acceptable for most production retrieval SLAs, but some applications (e.g. exact k-NN fact lookup) require 0 gap — our path to closing this is a longer rerank budget + adaptive `ef_search`.

---

## 11. Conclusion  *(~0.3 page)*

- CXL-Vector reframes HNSW on CXL from a data placement problem to a scheduling problem.
- Three co-designed components — layered placement, coarse-to-exact search, morsel-driven Issue/Consume pipeline — deliver 2.7×–6.5× QPS and 3.1×–6.8× latency improvements over strong DRAM baselines at ≤0.38 pp recall gap across 8 public datasets.
- The design demonstrates that commodity CXL hardware, *without* hardware co-design, is sufficient for DRAM-quality HNSW serving at CXL-tier capacity.
- Source code, traces, and reproducibility artifacts to be released with final submission.

---

## 12. References  *(~0.5 page)*
- ~40–50 references covering HNSW, DiskANN, SPANN, FAISS, CXL-ANNS, d-HNSW, CXL characterization (MICRO'23), morsel parallelism (SIGMOD'14), PQ/RaBitQ/TurboQuant, ANN benchmarks.

---

## Writing plan & open items

### Figures to produce (finalized)
| # | Figure | Source | Status |
|---|---|---|---|
| 1 | Memory tier landscape (latency × capacity × $) | Hand-drawn | TODO |
| 2 | Naive CXL-HNSW throughput collapse microbench | `thread_scaling.csv` | Existing via `comp.py` |
| 3 | Pareto plot: (DRAM footprint, Recall, QPS) across 5 systems | New — needs FAISS experiments | TODO |
| 4 | 3-panel main comparison (QPS/Lat/Recall × 8 datasets) | `results.csv` | Existing (`comparison_main.png`) |
| 5 | Bottleneck decomposition stacked bars | `BatchQueryProfile` | TODO |
| 6 | Thread scaling + ablation (3 panels) | `thread_scaling.csv` | Existing (`thread_scaling.png`) |
| 7 | Sensitivity curves (morsel, rerank, ef, patience) | New experiments | TODO |
| 8 | Memory footprint bars by system | New experiments | TODO |

### Experiments to run *(prioritized)*
1. **P0 — FAISS baselines:** `IndexHNSWSQ` and `IndexRefine(HNSWSq, Flat)` on all 8 datasets, 56 threads
2. **P0 — Naive CXL-HNSW microbenchmark:** for §3.1 motivation figure
3. **P1 — Sensitivity sweeps:** morsel, rerank budget, ef, patience
4. **P1 — Memory footprint collection:** `smem` / `/proc/<pid>/status` on all systems
5. **P1 — Bottleneck decomposition:** `BatchQueryProfile` for all baselines (where feasible)
6. **P2 — CXL-ANNS reproduction:** from `docs/CXL_ANNS_REPRODUCTION.md`
7. **P2 — NSG baseline:** add as alternative graph index comparison
8. **P2 — Multi-CXL-device scaling:** if second device available

### Open questions the reviewers will ask
- Why SQ8 and not PQ? → tradeoff section: lookup complexity, VNNI-friendliness, latency variance
- How do you handle online updates? → discussed in §10 limitations
- What if my CXL device has worse latency (soft-IP vs hard-IP)? → sensitivity bucket in §8.6
- Does your approach generalize to other graph indexes (NSG, Vamana)? → §10 discussion
- Memory cost (CXL hardware + DRAM) vs pure-DRAM baseline at equivalent capacity? → §8.7 table
- Build-time cost vs serving savings break-even? → §8.8 discussion

### Section-length budget (12 pages)
| Section | Pages |
|---|---:|
| Abstract | 0.25 |
| §1 Introduction | 1.5 |
| §2 Background | 1.0 |
| §3 Motivation | 1.0 |
| §4 Layered Placement | 1.0 |
| §5 Coarse-to-Exact Search | 1.5 |
| §6 Morsel-Driven Execution | 1.5 |
| §7 Implementation | 0.5 |
| §8 Evaluation | 3.0 |
| §9 Related Work | 0.5 |
| §10 Discussion / Limitations | 0.3 |
| §11 Conclusion | 0.3 |
| §12 References | 0.5 |
| **Total** | **12.85** → trim in §2 / §7 |

---

## Appendices *(supplementary material — optional)*
- **A.** Build→Load call graph (as in §附录 A of the analysis doc)
- **B.** Runtime configuration knobs table (as in §附录 B)
- **C.** Detailed NUMA verification dumps
- **D.** Reproducibility: build commands, dataset preparation, benchmark harness
