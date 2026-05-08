---
id: satellite-system-graph
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
  - raw/sources/papers/satellite-system-graph-2019.pdf
related:
  - nsg
  - monotonic-relative-neighborhood-graph
  - proximity-graph-theory-for-ann
  - approximate-nearest-neighbor-search
confidence: high
---

# Satellite System Graph

## Profile

Satellite System Graph (SSG) is a graph ANN method that selects outgoing edges so each node has more evenly distributed directional coverage. Its practical variant, NSSG, is designed to reduce the indexing cost of exact SSG.

## Why It Matters

- It is a direct follow-up to [NSG](nsg.md) and critiques NSG's sparsity.
- It frames graph search around MSNET-style monotonicity and explicitly distinguishes indexed from unindexed queries.
- It introduces a tunable sparsity/directional-coverage parameter, making graph degree a search-quality knob rather than only a memory budget.

## Limits

SSG/NSSG remains an in-memory graph-index design. It does not by itself solve storage-tier, CXL, GPU, or SSD layout issues.

## Related Pages

- [SSG Source Note](../source-notes/satellite-system-graph-2019.md)
- [NSG](nsg.md)
- [Monotonic Relative Neighborhood Graph](monotonic-relative-neighborhood-graph.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
