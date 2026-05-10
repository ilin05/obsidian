---
id: hm-ann-2020
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - heterogeneous-memory
  - graph-index
  - billion-scale
  - hnsw
source_count: 1
sources:
  - raw/sources/papers/hm-ann-2020.pdf
related:
  - second-tier-memory-for-vector-search
  - approximate-nearest-neighbor-search
  - hnsw
  - nsg
confidence: high
---

# HM-ANN: Efficient Billion-Point Nearest Neighbor Search on Heterogeneous Memory

Jie Ren, Minjia Zhang, and Dong Li, NeurIPS 2020 (UC Merced, Microsoft Research).

## Summary

HM-ANN enables billion-scale graph-based ANNS on a single machine without vector compression by leveraging heterogeneous memory (HM): fast-but-small DRAM + slow-but-large Optane PMM. It generalizes HNSW with a top-down insertion phase (building L0 in slow memory) and a bottom-up promotion phase (elevating pivot nodes to upper layers in fast memory). This hierarchical memory-aware placement ensures most search accesses hit fast memory while full-precision vectors remain available in slow memory. At billion-point scale, HM-ANN outperforms compression-based methods (L&C, IMI+OPQ) by 46% higher recall at matched latency, and is 2–5.8× faster than HNSW/NSG baselines adapted for HM.

## Key Contributions

- **HM-aware HNSW generalization**: Top-down insertion builds the largest layer (L0, containing all vectors) in slow memory; bottom-up promotion elevates pivot nodes to form upper layers in fast memory.
- **Dynamic migration**: Prefetches likely-to-be-accessed graph nodes from slow memory to fast memory during search.
- **Parallel search**: Overlaps search operations across fast and slow memory to hide PMM latency.
- **Performance model**: Analytical model to select hyperparameters (layer sizes, efSearch) that satisfy both latency and recall constraints.
- **HNSW and NSG HM baselines**: First implementations of HNSW and NSG on heterogeneous memory (first-touch NUMA and hardware-managed caching modes).

## Algorithm Details

### Construction
1. **Top-down phase**: Insert all vectors into L0 (slow memory), building navigable small-world connections.
2. **Bottom-up promotion**: Select pivot nodes from L0 that provide good coverage; promote them to L1 (fast memory). Repeat to build L2, L3, etc.
3. Upper layers are fully in fast memory; L0 is entirely in slow memory.

### Search
1. Start at top layer (fast memory) — standard 1-greedy search.
2. Descend layer by layer; most steps hit fast memory.
3. At L0 (slow memory): use dynamic migration to prefetch nodes likely to be accessed, parallel search to overlap computation.
4. Performance model guides efSearch per layer to meet latency budget.

## Reported Results

- 95% top-1 recall in <1ms on billion-scale datasets (BIGANN, DEEP1B)
- 46% higher recall than L&C and IMI+OPQ at same latency
- 2× faster than HNSW-HM baseline, 5.8× faster than NSG-HM baseline at 95% recall
- Full-precision distance computation (no compression artifacts)

## Significance

HM-ANN demonstrates that heterogeneous memory — specifically Intel Optane PMM — can break the accuracy-latency trade-off imposed by DRAM capacity limits. Rather than compressing vectors (and losing accuracy), HM-ANN selectively places data across memory tiers to preserve full-precision distance computation. This is a hardware-aware alternative to the compression-heavy approach dominant in billion-scale ANN.

## Limits And Open Questions

- Optane PMM has been discontinued by Intel; the approach needs re-validation on CXL-attached memory or other HM technologies.
- L0 traversal in slow memory still accounts for ~30% of search time; reducing PMM accesses further is an open problem.
- Construction time is significant (full HNSW build on billion points + promotion phase).
- Dynamic migration policy is dataset- and query-distribution-dependent.

## Related Pages

- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [HNSW](../entities/hnsw.md)
- [NSG](../entities/nsg.md)
