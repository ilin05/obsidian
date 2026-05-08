---
id: nsg
type: entity
status: active
created: 2026-04-08
updated: 2026-05-08
tags:
  - method
  - ann
  - graph-index
source_count: 5
sources:
  - raw/sources/papers/nsg-2019.pdf
  - raw/sources/papers/nn-descent-2011.pdf
  - raw/sources/papers/dpg-ann-experiments-2016.pdf
  - raw/sources/papers/monotonic-proximity-graphs-2021.pdf
  - raw/sources/papers/satellite-system-graph-2019.pdf
related:
  - nsg-2019
  - monotonic-proximity-graphs-2021
  - satellite-system-graph-2019
  - monotonic-relative-neighborhood-graph
  - satellite-system-graph
  - nn-descent
  - diversified-proximity-graph
  - relative-nn-descent
  - proximity-graph-theory-for-ann
  - hnsw
  - diskann
  - approximate-nearest-neighbor-search
confidence: high
---

# NSG

## Profile

Navigating Spreading-out Graph (NSG) is a practical graph index for ANN search, designed as an approximation to MRNG with strong focus on path quality, connectivity, and memory efficiency.

## Why It Matters

- It pushed graph-based ANN to stronger billion-scale practicality and was reported in production use.
- It serves as a key comparison point for later graph systems such as DiskANN.
- It is now the main bridge from practical ANN systems to [Monotonic Relative Neighborhood Graph](monotonic-relative-neighborhood-graph.md) theory in this vault.
- It depends on an approximate KNN graph/candidate-pool pipeline, so [NN-Descent](nn-descent.md) and [DPG](diversified-proximity-graph.md) are important predecessors for understanding its construction path.
- Later SSG/NSSG work critiques NSG as potentially too sparse and studies unindexed-query search guarantees.

## Related Pages

- [NSG Source Note](../source-notes/nsg-2019.md)
- [Monotonic Relative Neighborhood Graph](monotonic-relative-neighborhood-graph.md)
- [Satellite System Graph](satellite-system-graph.md)
- [NN-Descent](nn-descent.md)
- [Diversified Proximity Graph](diversified-proximity-graph.md)
- [Relative NN-Descent](relative-nn-descent.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
- [HNSW](hnsw.md)
- [DiskANN](diskann.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
