---
id: satellite-system-graph-2019
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - ssg
  - proximity-graph
source_count: 1
sources:
  - raw/sources/papers/satellite-system-graph-2019.pdf
related:
  - satellite-system-graph
  - nsg
  - monotonic-relative-neighborhood-graph
  - proximity-graph-theory-for-ann
confidence: high
---

# High Dimensional Similarity Search with Satellite System Graph

## Summary

Fu, Wang, and Cai propose Satellite System Graphs (SSG) and a practical variant, NSSG, for high-dimensional graph-based ANN. The paper argues that [NSG](../entities/nsg.md) is too sparse, lacks a guarantee for unindexed queries, and has costly edge selection. SSG addresses this by spreading each node's outgoing edges more evenly across directions while remaining in the monotonic-search-network family.

## Key Contributions

- Defines SSG as a new MSNET family with outgoing edges distributed omnidirectionally.
- Distinguishes indexed-query and unindexed-query behavior, which earlier graph ANN papers often blur.
- Introduces an angle hyperparameter to adjust graph sparsity and directional coverage.
- Proposes NSSG as a scalable approximation of exact SSG.
- Reports improved high-precision search performance and faster edge-selection than NSG in several benchmarks.

## Relation To NSG

SSG is best read as an NSG follow-up. NSG approximates MRNG and tries to sparsify aggressively. SSG argues that over-sparsification can harm search and that a more directionally balanced neighborhood can preserve monotonic-search behavior for both indexed and unindexed queries.

## Limits And Open Questions

- Exact SSG is still too expensive; practical NSSG depends on approximate construction.
- The paper is primarily in-memory and does not address SSD/CXL placement.
- NSSG may use more memory than non-graph methods, so it is most relevant when high recall and low latency justify graph-index cost.

## Related Pages

- [Satellite System Graph](../entities/satellite-system-graph.md)
- [NSG](../entities/nsg.md)
- [Monotonic Relative Neighborhood Graph](../entities/monotonic-relative-neighborhood-graph.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
