# Cross-Query Rerank Coalescing — Implementation Plan

**Working title of §6.x subsection:** *Bandwidth-aware Cross-Query Rerank Coalescing*
**Target:** add a real CXL-specific algorithmic mechanism to CXL-Vector to strengthen novelty
**Estimated effort:** 2–4 weeks (1 person)
**Status:** design 2026-04-11

---

## 1. Core idea in one paragraph

In the current CXL-Vector rerank stage, each query in a morsel independently pops its top-*b* rerank candidates from its own heap and independently issues raw-vector fetches to CXL. When two queries in the same morsel share a rerank candidate (same `node_id`), they trigger **two independent CXL fetches for the same 512-byte float vector**. Since the rerank stage is the only code path that touches CXL, every duplicate fetch is pure PCIe waste. *Cross-query rerank coalescing* rewrites the rerank phase as a morsel-level operation: collect the union of rerank candidates across all queries in the morsel, fetch each unique `node_id` exactly once, and scatter the fetched vector to every query that needs it for exact distance computation.

## 2. Why this is a legitimate research contribution (not just engineering)

This is the argument you need ready for reviewers. Write it down now so you don't have to reconstruct it in §6 later:

1. **It is CXL-specific by design.**
   - On pure DRAM, the savings are ~0 because raw vector access is ~80 ns and L2 already handles locality. No one bothers.
   - On SSD, the fetch granularity is a 4 KB page; cross-query dedup is subsumed by the page cache.
   - Only on CXL does cross-query dedup win: access is per-cache-line (64 B), PCIe is the bottleneck resource, and the 512-byte vector is small enough that batching many requests into one is worth the dedup bookkeeping.

2. **It is structurally enabled by morsel-driven batch execution.**
   - Single-query systems (FAISS IndexRefine, DiskANN serving, hnswlib) literally cannot do this — they don't have a batch abstraction at the rerank stage.
   - CXL-ANNS's hardware offload sidesteps it (EPs compute distances locally, dedup is moot).
   - Only in a batch-scheduled CXL system does this mechanism become possible.
   - This is the tightest novelty framing: *"our scheduler's batch-locality creates a new optimization surface that no prior system could access."*

3. **It is not a trivial optimization.**
   - It changes the data flow from per-query scatter to per-candidate scatter
   - It requires a new scatter/gather distance kernel that computes exact distance for multiple queries against one vector
   - It forces a re-examination of when the overhead (dedup bookkeeping) pays for the savings (PCIe fetches saved)

4. **There is a real tradeoff to analyze (good for §6 + §8).**
   - Dedup bookkeeping (hash or sort) has per-morsel cost
   - Overlap rate varies with query distribution — worst case (uniform queries) the savings are small; best case (clustered / skewed queries) the savings are large
   - The existence of the tradeoff is what makes it a *research mechanism*, not a *micro-optimization*

---

## 3. Algorithmic sketch

### 3.1 Current per-query rerank (conceptual)

```text
for each query q in morsel:
    rerank_ids = q.top_candidates.nth_element(budget)
    storage.prefetchVecBatch(rerank_ids)
    for id in rerank_ids:
        raw_vec = storage.getVec(id)            # CXL fetch
        d_exact = exact_distance(q.vec, raw_vec)
        q.update_top_k(id, d_exact)
```

Per-morsel CXL fetch count: `|morsel| × budget`.

### 3.2 Coalesced rerank (proposed)

```text
# Phase 1: Collect — each query contributes its rerank candidates
plan = MorselRerankPlan()
for each query q in morsel:
    for id in q.top_candidates.nth_element(budget):
        plan.add(id, q.index)         # append (node_id, query_idx)

# Phase 2: Deduplicate — build unique node list + owner map
plan.finalize()   # sort by node_id, run-length group owners
# result: plan.unique_nodes[]          — size ≤ |morsel| × budget
#         plan.owner_offsets[]         — CSR offsets into owners[]
#         plan.owners[]                — (query_idx) per unique node

# Phase 3: Prefetch the deduplicated set in one batch
storage.prefetchVecBatch(plan.unique_nodes)

# Phase 4: Scatter-compute — one fetch → many distance computations
for u_idx in range(plan.unique_nodes.size):
    node_id = plan.unique_nodes[u_idx]
    raw_vec = storage.getVec(node_id)         # at most one CXL fetch per unique id
    for o_idx in range(plan.owner_offsets[u_idx], plan.owner_offsets[u_idx+1]):
        q_idx = plan.owners[o_idx]
        q = morsel[q_idx]
        d_exact = exact_distance(q.vec, raw_vec)
        q.update_top_k(node_id, d_exact)
```

Per-morsel CXL fetch count: `|unique_nodes| ≤ |morsel| × budget`.

**Savings = 1 − |unique_nodes| / (|morsel| × budget)**. This is the metric you want in the paper.

### 3.3 Data structure choice

Two viable options. Benchmark both.

**Option A — Sort + run-length group (simple, cache-friendly):**
- Keep a scratch `std::vector<std::pair<uint32_t, uint16_t>>` of `(node_id, query_idx)` pairs
- `std::sort` by `node_id` (stable) — for `|morsel| × budget ≈ 128` pairs, this is ~1 µs
- Linear scan produces unique node list + CSR owner layout in one pass

**Option B — Open-addressed hash table (faster in principle, less predictable):**
- Fixed-size inline hash table (256 slots, linear probe)
- Insert (node_id) → if hit, append query_idx to owner list; if miss, allocate new entry
- Amortized O(1) per insert but with branch mispredictions

**Recommendation:** start with Option A. It's simpler, the `128` collection is tiny so sort cost is noise, and sorted order gives you a free prefetch-ordering optimization (sequential by node_id → better CXL locality if vectors are laid out in a nearby range).

### 3.4 Scatter-compute distance kernel

The current `exact_distance_dispatch` takes one query vector and one data vector. For coalesced rerank you want a kernel signature like:

```cpp
void exact_distance_1_to_M(
    const float* data_vec,          // one raw vec from CXL (now hot in L2)
    const float* const* query_vecs, // M query vectors (DRAM, hot)
    size_t M,
    float* out_distances            // M outputs
);
```

For M ≤ 4 (which is the expected common case at morsel_size=2 and modest overlap), this is essentially 1–4 dot products against the same data vector. Important: **the data vector stays in L2 for all M computations** — that's the win.

You can implement this with:
- AVX-512: load `data_vec` once into zmm registers, loop over query vectors, compute dot products
- Keep existing 128d / 200d / 768d specializations, just add a second loop dimension

---

## 4. Implementation phases

### Phase 0 — Baseline measurement (1–2 days) **[do this first]**

Before writing any code, **measure the overlap rate** on your existing 8 datasets with morsel_size ∈ {1, 2, 4, 8, 16}.

Add a profile counter to the current rerank phase that logs:
- Per-morsel: total rerank candidate count, unique count (simulated — sort + dedup just for counting, don't actually coalesce)
- Aggregate: distribution of `overlap_ratio = 1 − unique/total`

**Why this comes first:** if the overlap rate is < 3% on all 8 datasets, the optimization is not worth implementing and you need to pivot to a different mechanism. If it's 5–20%, it's worth doing. If it's > 20%, you've found a really strong result.

Deliverable: a table showing overlap rate × morsel size × dataset. This table goes straight into §8 of the paper regardless of whether coalescing ends up as a win.

### Phase 1 — Scaffolding (2–3 days)

- Add runtime flag: `EXP_RERANK_COALESCE` (default off initially)
- Add `MorselRerankPlan` struct in `include/index/batch.h` alongside QueryState
- Add per-morsel thread-local scratch space (the 128-entry pair vector) — preallocated at scheduler setup time, not per-morsel
- Do not yet change the rerank code path; just build and sanity-check the scaffolding

### Phase 2 — Plan construction (2–3 days)

- Implement `plan.add()` and `plan.finalize()` with Option A (sort + group)
- Unit test: given a synthetic set of (node_id, query_idx) pairs, verify unique_nodes, owner_offsets, owners match expectation
- Benchmark the standalone plan construction cost on representative morsel sizes — it should be < 500 ns at morsel_size=2, budget=32

### Phase 3 — Scatter-compute kernel (3–4 days)

- Implement `exact_distance_1_to_M` for L2 and IP metrics, 128d / 200d / 768d specializations, AVX-512 path
- Fall back to calling `exact_distance_dispatch` in a loop for the scalar path — correctness over performance
- Unit test: numerical equivalence with existing per-query kernel on random vectors

### Phase 4 — Integration (3–4 days)

- Replace the per-query rerank loop in `batch.h` with the coalesced version behind the `EXP_RERANK_COALESCE` flag
- Wire the plan construction into the existing scheduler's rerank phase
- Run full SIFT-10M test with and without coalescing — **verify that Recall@10 is bit-exact identical**. Coalescing should be a pure throughput optimization with zero accuracy impact. If Recall changes, there is a bug.

### Phase 5 — Ablation experiments (2–3 days)

Run the existing benchmark suite with the flag on/off:
- All 8 datasets at 56 threads, morsel=2, default rerank budget
- SIFT-10M thread sweep (the existing figure) with and without coalescing
- SIFT-10M morsel sweep {1, 2, 4, 8, 16} with coalescing on (at morsel=1 savings should be 0 — sanity check)

Collect:
- QPS delta (absolute and relative)
- PCIe bandwidth usage via `pcm-memory` or `perf stat -e uncore_iio_*` — should show a clear drop
- Per-query latency distribution (expect lower mean, similar P99)
- Overlap rate histogram per dataset

### Phase 6 — Writing (2 days)

Write §6.x *Bandwidth-aware Cross-Query Rerank Coalescing*. Structure:
- 6.x.1 Observation: overlap rate table (your Phase 0 result)
- 6.x.2 Algorithm: the scatter-compute rewrite
- 6.x.3 Why it only works in the CXL-Vector architecture (the novelty argument from §2 above)
- 6.x.4 Complexity analysis and break-even point
- Add to §8 evaluation: ablation row for coalescing on/off

---

## 5. What to measure (the paper-facing numbers)

1. **Overlap rate by dataset + morsel size** — the raw empirical observation
2. **QPS improvement from coalescing** — goal: ≥ 8% at morsel=2, ≥ 15% at morsel=4
3. **PCIe bandwidth reduction** — direct proof of the mechanism via hardware counters
4. **Recall invariance** — bit-exact proof coalescing is not cheating
5. **Overhead breakdown** — what fraction of per-query time is dedup bookkeeping? Must be < 5%

---

## 6. Risks, and what to do if they materialize

| Risk | Symptom | Mitigation |
|---|---|---|
| Overlap rate too low (< 3%) on all datasets | Phase 0 table shows no opportunity | Pivot to a different mechanism. My next recommendation would be *adaptive rerank budget driven by measured CXL pressure* — a different angle on the same "CXL-specific scheduling" contribution |
| Overlap is high but savings are eaten by dedup overhead | QPS improvement < 3% despite overlap rate ≥ 10% | Switch from Option A (sort) to Option B (open-addressed hash) — constant-factor improvement. Also: preallocate scratch, no per-morsel malloc |
| Scatter-compute kernel is slower than per-query kernel | Per-call latency regresses even at M=1 | Keep existing kernel as fast path for M=1, only use scatter kernel for M ≥ 2 |
| Recall changes after coalescing | Bit-exact comparison fails | Almost certainly a bug in the owner-scatter indexing. Coalescing is mathematically a no-op on Recall. Debug with a single morsel worth of data |
| P99 latency regresses | Tail improves for some queries, degrades for others | The scatter-compute phase serializes work within a morsel — if one query has high M, all queries wait. Mitigation: set a hard cap on M per unique node (e.g., ≤ 4); above that, fall back to per-query |
| Only one or two datasets show gains | E.g., clustered data benefits, uniform does not | This is actually fine for the paper — it becomes a *workload sensitivity* story: "coalescing wins on clustered and production-like queries, is neutral on synthetic uniform benchmarks" |

---

## 7. Open design questions to decide during Phase 1

1. **Where does coalescing live in the code?** Inside `batch.h` rerank phase, or as a separate module invoked from it? — Recommendation: start inline, refactor only if §6 needs a clean abstraction for the paper.

2. **Does the scheduler need any changes?** — No. Coalescing is morsel-local. The scheduler just keeps issuing morsels; the change is entirely inside the rerank stage.

3. **Interaction with `rerank_abs_limit` and `rerank_factor`?** — Coalescing does not change these. Each query still has the same per-query rerank budget; coalescing only deduplicates the *fetch*.

4. **Interaction with the deletion bitmap?** — None. Deletion check happens before candidates enter rerank, at which point dedup operates on already-valid node IDs.

5. **Thread safety?** — None needed. Each worker thread has its own morsel and its own plan scratch. No cross-worker sync.

---

## 8. Decision point after Phase 0

Phase 0 is a 1–2 day spike that tells you whether to continue. Three possible outcomes:

- **Overlap ≥ 10% on ≥ 4 of 8 datasets:** proceed to Phase 1–6, confident this is a real contribution
- **Overlap 3–10%, mixed across datasets:** proceed but with a scaled-down scope — maybe only write it up as a minor §6.x subsection rather than a full contribution
- **Overlap < 3% on all datasets:** stop. Pivot to adaptive rerank budget or query-difficulty-aware morsel sizing. Phase 0 itself is still a useful paper result — "we measured cross-query overlap on CXL-resident rerank and found it to be ≤ X% on uniform benchmarks, motivating [other mechanism]"

Either way, Phase 0 is cheap insurance that prevents 3 weeks of wasted implementation.

---

## 9. What this buys you in the paper

- A concrete algorithmic mechanism that **only** works in the CXL-Vector architecture (nowhere else in the design space can coalesce fetches this way)
- A new subsection in §6 that reads as algorithmic research rather than engineering integration
- An additional axis for the evaluation section: dedup rate analysis per dataset, bandwidth reduction graph, ablation with the flag off
- A clean answer to the reviewer question *"what is CXL-specific about your contribution?"*
- A new bullet in the contributions list of §1: *"A bandwidth-aware cross-query rerank coalescing mechanism that exploits morsel-local batch structure to eliminate duplicate CXL fetches, reducing PCIe traffic by X% on workloads with clustered query distributions."*
