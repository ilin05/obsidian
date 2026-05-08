---
id: navigable-small-world-graph
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - method
  - ann
  - graph-index
  - small-world
source_count: 2
sources:
  - raw/sources/papers/kleinberg-small-world-2000.pdf
  - raw/sources/papers/navigable-small-world-graph-ann-2014.pdf
related:
  - hnsw
  - proximity-graph-theory-for-ann
  - approximate-nearest-neighbor-search
confidence: high
---

# Navigable Small World Graph

## Profile

Navigable Small World (NSW) graph search is an early graph-based ANN approach that uses greedy routing over a proximity graph with long-range links. Malkov et al. built an ANN index by retaining approximate Delaunay links from incremental construction, yielding local proximity edges plus older long-range shortcuts.

## Why It Matters

- It is the direct predecessor of [HNSW](hnsw.md).
- It connects ANN graph search to the broader small-world navigability problem: useful long links must be discoverable by a local search procedure.
- It provides the historical bridge from Delaunay/Voronoi greedy routing intuition to modern learned-vector ANN indexes.

## Limits

NSW lacks HNSW's explicit hierarchy and later high-recall neighbor-selection refinements. It is mostly useful here as a predecessor and conceptual bridge rather than a current baseline.

## Related Pages

- [NSW Source Note](../source-notes/navigable-small-world-graph-ann-2014.md)
- [Kleinberg Small-World Source Note](../source-notes/kleinberg-small-world-2000.md)
- [HNSW](hnsw.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
