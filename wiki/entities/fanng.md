---
id: fanng
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - method
  - ann
  - graph-index
  - proximity-graph
source_count: 1
sources:
  - raw/sources/papers/fanng-2016.pdf
related:
  - approximate-nearest-neighbor-search
  - proximity-graph-theory-for-ann
  - monotonic-relative-neighborhood-graph
  - nsg
  - ngt-onng
confidence: high
---

# FANNG

## Profile

FANNG is a graph ANN method that prunes candidate neighbors through an occlusion-style rule. It keeps edges that are useful for greedy search while suppressing locally redundant links.

## Why It Matters

- It is an RNG-like pruning predecessor in the graph ANN lineage.
- It helps explain why proximity-graph indexes care about directional diversity and occlusion, not only nearest-neighbor rank.
- It provides a useful contrast to [NSG](nsg.md), SSG/NSSG, and Vamana-style pruning.

## Limits

FANNG is best read as a practical proximity-graph pruning method. Later MRNG/SSG work gives a cleaner theory layer, and later systems give broader benchmark and hardware coverage.

## Related Pages

- [FANNG Source Note](../source-notes/fanng-2016.md)
- [Monotonic Relative Neighborhood Graph](monotonic-relative-neighborhood-graph.md)
- [NSG](nsg.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
