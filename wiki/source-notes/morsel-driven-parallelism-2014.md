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
  - raw/sources/papers/morsel-driven-parallelism-2014.pdf
related:
  - morsel-driven-parallelism
confidence: high
---

# Morsel-Driven Parallelism: A NUMA-Aware Query Evaluation Framework for the Many-Core Age

## Summary

This paper introduces morsel-driven query execution for many-core NUMA systems: small morsels of work are dynamically scheduled to workers that execute entire pipelines, enabling elasticity, load balancing, and NUMA-aware locality.

## Main Ideas To Keep

- Parallelism is assigned at runtime rather than baked statically into the plan.
- Morsels allow elasticity, work stealing, and dynamic balancing across uneven work.
- NUMA-aware scheduling and data placement are central, not secondary, design concerns.

## Relationship To Vector Search Systems

- The paper is not about vector search, but its scheduling vocabulary is useful for batched ANN systems.
- The transferable idea is dynamic assignment of small work units to reduce load imbalance and improve locality.
- It is most relevant when query difficulty varies or when a system needs to overlap compute with memory stalls.

## Related Pages

- [Morsel-driven Parallelism](../entities/morsel-driven-parallelism.md)
