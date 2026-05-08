---
id: nn-descent
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
  - raw/sources/papers/nn-descent-2011.pdf
related:
  - approximate-nearest-neighbor-search
  - proximity-graph-theory-for-ann
  - relative-nn-descent
  - diversified-proximity-graph
  - nsg
confidence: high
---

# NN-Descent

## Profile

NN-Descent is an approximate kNN graph construction method based on iterative neighbor propagation. It uses the observation that a neighbor of a neighbor is likely to be a neighbor, which lets it improve a KNN graph without exhaustive all-pairs comparison.

## Why It Matters

- It is the core KNN graph construction primitive behind KGraph-style baselines.
- It supplies candidate graphs for later pruning methods such as [DPG](diversified-proximity-graph.md), [NSG](nsg.md), NSSG/SSG, and Relative NN-Descent.
- It works with generic similarity measures, making it broader than Euclidean-only tree initialization.
- It clarifies that graph ANN has two separable costs: building a usable neighbor graph and searching on the final graph.

## Limits

NN-Descent optimizes graph construction. A high-recall approximate KNN graph is not automatically the best search graph, because search also depends on entry selection, pruning, connectivity, and routing.

## Related Pages

- [NN-Descent Source Note](../source-notes/nn-descent-2011.md)
- [Relative NN-Descent](relative-nn-descent.md)
- [Diversified Proximity Graph](diversified-proximity-graph.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
