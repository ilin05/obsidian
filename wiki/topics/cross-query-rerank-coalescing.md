---
id: cross-query-rerank-coalescing
type: topic
status: seed
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - cxl
  - rerank
  - scheduling
source_count: 1
sources:
  - cross-query-rerank-coalescing-plan.md
related:
  - cxl-vector
  - cxl-vector-commodity-cxl-hnsw-serving
  - morsel-driven-parallelism
confidence: medium
---

# Cross-Query Rerank Coalescing

## Summary

Cross-query rerank coalescing is a proposed CXL-specific mechanism for CXL-Vector. Instead of letting each query independently fetch its rerank candidates from CXL, a morsel collects all rerank candidates, deduplicates node IDs, fetches each unique raw vector once, and scatters the exact-distance computation back to every query that needs that vector.

## Why It Matters

The rerank stage is the only stage that should touch raw CXL-resident vectors. If two queries in the same morsel rerank the same node, duplicate raw-vector fetches waste PCIe bandwidth. Coalescing turns morsel-local batch structure into a bandwidth optimization surface.

## Algorithm Sketch

1. Collect `(node_id, query_idx)` pairs from each query's rerank shortlist.
2. Sort by `node_id` and group identical IDs.
3. Build `unique_nodes`, `owner_offsets`, and `owners`.
4. Prefetch each unique node once from CXL.
5. Compute exact distance from the fetched raw vector to each owner query.
6. Update each query's final top-k exactly as the non-coalesced path would.

The key metric is:

```text
savings = 1 - |unique_nodes| / (morsel_size * rerank_budget)
```

## Research Status

This is a design candidate, not an established result. The first required measurement is overlap rate across datasets and morsel sizes. If overlap is consistently below 3%, the mechanism should not be implemented as a major contribution. If overlap reaches 5-20% on realistic workloads, it becomes a strong paper mechanism.

## Paper Value

- It is specific to CXL because raw vectors are cache-line-addressable and PCIe bandwidth is scarce.
- It depends on morsel-local batch execution, so it is structurally enabled by CXL-Vector's scheduler.
- It provides a measurable bandwidth metric independent of recall.
- It should preserve recall bit-for-bit; any recall change indicates an implementation bug.

## Risks

- Uniform query workloads may have little candidate overlap.
- Dedup bookkeeping may cost more than the saved CXL fetches.
- Scatter-compute can serialize work inside a morsel and hurt tail latency if owner groups become large.

## Related Pages

- [CXL-Vector](../entities/cxl-vector.md)
- [Commodity CXL HNSW Serving](cxl-vector-commodity-cxl-hnsw-serving.md)
- [Morsel-driven Parallelism](../entities/morsel-driven-parallelism.md)

