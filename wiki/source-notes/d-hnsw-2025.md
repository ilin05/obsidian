---
id: d-hnsw-2025
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - ann
  - disaggregated-memory
  - rdma
  - hnsw
source_count: 1
sources:
  - raw/sources/papers/d-hnsw-2025.pdf
related:
  - d-hnsw
  - hnsw
  - disaggregated-memory-vector-search
  - approximate-nearest-neighbor-search
confidence: medium
---

# Efficient Vector Search on Disaggregated Memory with d-HNSW

## Summary

This paper proposes d-HNSW, an RDMA-based disaggregated-memory vector search system that restructures HNSW serving around compute/memory pool separation. The focus is reducing network round trips and data-movement overhead in greedy graph traversal.

## Key Contributions

- Proposes representative-index caching to keep critical lightweight routing structure near compute.
- Proposes RDMA-friendly index/data layout for fewer round trips and better insertion/read behavior.
- Proposes batched query-aware data loading to reduce duplicate partition transfers under limited cache capacity.

## Reported Results

- Reports up to 117x lower latency than a naive disaggregated baseline while maintaining recall around 0.86-0.87 on SIFT1M in shown settings.
- Positions itself as an early RDMA-native vector search engine design for disaggregated memory.

## Limits And Open Questions

- This source is currently an arXiv paper and should be treated as evolving evidence.
- Reported gains are baseline-dependent and need broader head-to-head comparisons against other modern disaggregated ANN systems.

## Related Pages

- [d-HNSW](../entities/d-hnsw.md)
- [HNSW](../entities/hnsw.md)
- [Disaggregated Memory Vector Search](../topics/disaggregated-memory-vector-search.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
