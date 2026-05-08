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
  - raw/sources/papers/morsel-driven-parallelism-2014.pdf
related:
  - morsel-driven-parallelism-2014
confidence: high
---

# Morsel-driven Parallelism

## Profile

Morsel-driven parallelism is a runtime scheduling idea from database query processing in which work is broken into small morsels that are dynamically assigned to workers to improve load balance, elasticity, and NUMA locality.

## Why It Matters Here

- It is useful when thinking about batched vector search systems whose queries have variable work.
- More generally, it gives vocabulary for dynamic work assignment, load balancing, and NUMA-aware scheduling.

## Related Pages

- [Morsel-driven Parallelism Source Note](../source-notes/morsel-driven-parallelism-2014.md)
