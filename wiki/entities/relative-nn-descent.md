---
id: relative-nn-descent
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - method
  - ann
  - graph-index
  - construction
source_count: 1
sources:
  - raw/sources/papers/relative-nn-descent-2023.pdf
related:
  - nn-descent
  - nsg
  - hnsw
  - monotonic-relative-neighborhood-graph
  - proximity-graph-theory-for-ann
confidence: high
---

# Relative NN-Descent

## Profile

Relative NN-Descent is a graph-index construction method that fuses NN-Descent-style candidate propagation with RNG Strategy pruning. It aims to construct a search-effective graph directly and faster than refinement-based graph pipelines.

## Why It Matters

- It targets build time, a practical bottleneck for graph ANN indexes.
- It is now grounded in the original [NN-Descent](nn-descent.md) source note in this vault.
- It connects the RNG-pruning idea used by NSG/HNSW-like methods with NN-Descent construction.
- It is relevant for workloads where indexes are rebuilt or retuned often.

## Related Pages

- [Relative NN-Descent Source Note](../source-notes/relative-nn-descent-2023.md)
- [NN-Descent](nn-descent.md)
- [NSG](nsg.md)
- [HNSW](hnsw.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
