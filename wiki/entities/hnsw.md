---
id: hnsw
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - method
  - ann
  - graph-index
source_count: 4
sources:
  - raw/sources/papers/kleinberg-small-world-2000.pdf
  - raw/sources/papers/navigable-small-world-graph-ann-2014.pdf
  - raw/sources/papers/hnsw-2016.pdf
  - raw/sources/papers/patience-in-proximity-2025.pdf
related:
  - navigable-small-world-graph
  - proximity-graph-theory-for-ann
  - hnsw-2016
  - navigable-small-world-graph-ann-2014
  - kleinberg-small-world-2000
  - patience-in-proximity
  - nsg
  - diskann
  - d-hnsw
  - approximate-nearest-neighbor-search
confidence: high
---

# HNSW

## Profile

Hierarchical Navigable Small World (HNSW) is a graph-based ANN indexing method that uses multi-layer proximity graphs to route quickly from coarse to fine neighborhoods.

## Why It Matters

- It became a core baseline and building block for modern ANN systems.
- It extends the earlier [Navigable Small World Graph](navigable-small-world-graph.md) line by adding hierarchy and stronger neighbor selection.
- Later systems either optimize HNSW-like graph behavior or position against it on recall-latency-memory trade-offs.
- Recent work such as [Patience in Proximity](patience-in-proximity.md) explores low-overhead early-termination heuristics inside HNSW traversal.

## Related Pages

- [HNSW Source Note](../source-notes/hnsw-2016.md)
- [Navigable Small World Graph](navigable-small-world-graph.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
- [Patience in Proximity](patience-in-proximity.md)
- [NSG](nsg.md)
- [DiskANN](diskann.md)
- [d-HNSW](d-hnsw.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
