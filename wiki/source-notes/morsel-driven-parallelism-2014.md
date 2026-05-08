---
id: morsel-driven-parallelism-2014
type: source-note
status: active
created: 2026-04-08
updated: 2026-05-08
tags:
  - systems
  - scheduling
  - numa
  - morsel
source_count: 1
sources:
  - raw/morsel.pdf
related:
  - morsel-driven-parallelism
  - cxl-vector-commodity-cxl-hnsw-serving
  - cross-query-rerank-coalescing
confidence: high
---

# Morsel-Driven Parallelism: A NUMA-Aware Query Evaluation Framework for the Many-Core Age

## Summary

This paper introduces morsel-driven query execution for many-core NUMA systems: small morsels of work are dynamically scheduled to workers that execute entire pipelines, enabling elasticity, load balancing, and NUMA-aware locality.

## Main Ideas To Keep

- Parallelism is assigned at runtime rather than baked statically into the plan.
- Morsels allow elasticity, work stealing, and dynamic balancing across uneven work.
- NUMA-aware scheduling and data placement are central, not secondary, design concerns.

## Relationship To CXL-Vector

- CXL-Vector does not reuse this design verbatim from relational query processing.
- It adapts the morsel idea into batched ANN query scheduling over per-query state machines, remote-memory stalls, and rerank-heavy execution.
- The new cross-query rerank coalescing idea uses morsel-local query groups as a bandwidth optimization surface.

## Related Pages

- [Morsel-driven Parallelism](../entities/morsel-driven-parallelism.md)
- [Commodity CXL HNSW Serving](../topics/cxl-vector-commodity-cxl-hnsw-serving.md)
- [Cross-Query Rerank Coalescing](../topics/cross-query-rerank-coalescing.md)
