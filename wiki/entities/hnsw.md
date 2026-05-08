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
source_count: 2
sources:
  - raw/HNSW.pdf
  - raw/patience.pdf
related:
  - hnsw-2016
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
- Later systems either optimize HNSW-like graph behavior or position against it on recall-latency-memory trade-offs.
- Recent work such as [Patience in Proximity](patience-in-proximity.md) explores low-overhead early-termination heuristics inside HNSW traversal.

## Related Pages

- [HNSW Source Note](../source-notes/hnsw-2016.md)
- [Patience in Proximity](patience-in-proximity.md)
- [NSG](nsg.md)
- [DiskANN](diskann.md)
- [d-HNSW](d-hnsw.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
