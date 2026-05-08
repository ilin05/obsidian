---
id: diversified-proximity-graph
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - method
  - ann
  - graph-index
  - pruning
source_count: 1
sources:
  - raw/sources/papers/dpg-ann-experiments-2016.pdf
related:
  - approximate-nearest-neighbor-search
  - ann-benchmarking-methodology
  - nn-descent
  - nsg
  - proximity-graph-theory-for-ann
confidence: high
---

# Diversified Proximity Graph

## Profile

Diversified Proximity Graph (DPG) is a graph ANN method proposed in a broad empirical study of high-dimensional ANNS algorithms. It improves a neighborhood graph by diversifying edges so that search has better routes than a plain nearest-neighbor graph.

## Why It Matters

- It is a bridge between KGraph/NN-Descent-style approximate KNN graphs and later NSG/NSSG/Vamana-style pruning.
- It connects benchmarking methodology with graph design because it appears inside a comprehensive empirical study.
- It is useful for understanding why graph quality metrics and search performance can diverge.

## Limits

DPG should be separated from the broader 2016 benchmarking paper. The source ingested here is the arXiv v1.0 paper, so final publication metadata should be checked before citation.

## Related Pages

- [DPG Source Note](../source-notes/dpg-ann-experiments-2016.md)
- [NN-Descent](nn-descent.md)
- [NSG](nsg.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
