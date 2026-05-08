---
id: monotonic-proximity-graphs-2021
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - proximity-graph
  - mrng
source_count: 1
sources:
  - raw/sources/papers/monotonic-proximity-graphs-2021.pdf
related:
  - monotonic-relative-neighborhood-graph
  - nsg
  - satellite-system-graph
  - proximity-graph-theory-for-ann
confidence: high
---

# Understanding and Generalizing Monotonic Proximity Graphs for Approximate Nearest Neighbor Search

## Summary

Zhu and Zhang analyze Monotonic Relative Neighborhood Graphs (MRNG) as a theoretical model for graph-based ANN. The paper proves MRNG properties more explicitly, defines generalized MRNG variants, studies degree-bounded behavior, and introduces "conflicting nodes" as a possible way to escape local minima in approximate graphs.

## Key Contributions

- Proves that MRNG is a uniquely defined edge-minimal monotonic graph for a dataset.
- Defines generalized MRNG through degree bounds and restricted candidate pools, with [NSG](../entities/nsg.md) treated as a special generalized MRNG.
- Shows empirically that degree-bounded MRNG can preserve search accuracy while reducing construction and traversal cost.
- Studies MRNG degree distribution and highlights how high dimension increases degree pressure.
- Introduces conflicting nodes as a hidden structure that may help search escape local minima in generalized MRNGs.

## Importance

The paper turns the NSG/MRNG connection from a mostly design-motivation statement into a clearer theoretical object. It is especially useful for explaining why graph ANN pruning is not only a memory-saving heuristic: the goal is to preserve enough monotonic navigability while bounding degree and construction cost.

## Limits And Open Questions

- Exact MRNG construction remains too expensive for large-scale indexing.
- Experiments on exact MRNG are necessarily limited by construction cost.
- Conflicting nodes are presented as a theoretical direction, not a mature practical mechanism.
- The paper focuses on Euclidean distance and does not resolve hardware-tier behavior.

## Related Pages

- [Monotonic Relative Neighborhood Graph](../entities/monotonic-relative-neighborhood-graph.md)
- [NSG](../entities/nsg.md)
- [Satellite System Graph](../entities/satellite-system-graph.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
