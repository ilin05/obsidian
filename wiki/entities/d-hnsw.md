---
id: d-hnsw
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - method
  - ann
  - hnsw
  - rdma
  - disaggregated-memory
source_count: 1
sources:
  - raw/sources/papers/d-hnsw-2025.pdf
related:
  - d-hnsw-2025
  - hnsw
  - disaggregated-memory-vector-search
  - approximate-nearest-neighbor-search
confidence: medium
---

# d-HNSW

## Profile

d-HNSW is an RDMA-disaggregated adaptation of HNSW serving that restructures index/data access for lower network round trips and better batch behavior.

## Why It Matters

- It is a direct example of adapting ANN graph search to compute-memory disaggregation.
- It connects classic HNSW graph search with systems-level RDMA layout/transport design.

## Related Pages

- [d-HNSW Source Note](../source-notes/d-hnsw-2025.md)
- [HNSW](hnsw.md)
- [Disaggregated Memory Vector Search](../topics/disaggregated-memory-vector-search.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
