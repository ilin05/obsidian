---
id: morsel-driven-parallelism
type: entity
status: active
created: 2026-04-08
updated: 2026-05-08
tags:
  - concept
  - scheduling
  - numa
  - systems
source_count: 1
sources:
  - raw/morsel.pdf
related:
  - morsel-driven-parallelism-2014
  - cxl-vector-commodity-cxl-hnsw-serving
  - cross-query-rerank-coalescing
confidence: high
---

# Morsel-driven Parallelism

## Profile

Morsel-driven parallelism is a runtime scheduling idea from database query processing in which work is broken into small morsels that are dynamically assigned to workers to improve load balance, elasticity, and NUMA locality.

## Why It Matters Here

- It provides the conceptual ancestor for CXL-Vector's morsel-oriented batch scheduler.
- The current ANN runtime adapts this idea to per-query state, remote-memory stalls, and bounded rerank rather than relational pipelines.
- The proposed cross-query rerank coalescing mechanism depends on the same morsel-local batch structure.

## Related Pages

- [Morsel-driven Parallelism Source Note](../source-notes/morsel-driven-parallelism-2014.md)
- [Commodity CXL HNSW Serving](../topics/cxl-vector-commodity-cxl-hnsw-serving.md)
- [Cross-Query Rerank Coalescing](../topics/cross-query-rerank-coalescing.md)
