---
id: efanna-2016
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - knn-graph
  - construction
  - tree-index
source_count: 1
sources:
  - raw/sources/papers/efanna-2016.pdf
related:
  - efanna
  - nn-descent
  - flann
  - approximate-nearest-neighbor-search
confidence: high
---

# EFANNA: An Extremely Fast Approximate Nearest Neighbor Search Algorithm Based on kNN Graph

## Summary

Fu and Cai propose EFANNA, a graph ANN method that combines randomized hierarchical tree structures with approximate kNN graph construction and query-time graph refinement. The key idea is to use trees to produce good initial candidates, then use neighbor expansion over a graph to improve both construction and search.

## Key Contributions

- Uses multiple randomized truncated KD-trees to initialize approximate kNN graph construction.
- Refines the graph with NN-Descent-style neighbor propagation, reducing dependence on purely random starts.
- Uses the same tree structures to acquire query seeds before graph traversal.
- Argues that an approximate, low-recall kNN graph can still support effective ANN search because many wrong edges are only slightly farther than exact neighbors.

## Why It Matters

EFANNA sits between tree-based baselines such as [FLANN](../entities/flann.md) and graph-based methods such as [NSG](../entities/nsg.md). It is useful for separating two effects that later papers often mix: better graph construction through seed quality, and better search through graph routing.

## Limits And Open Questions

- The method is primarily an in-memory algorithm and does not address SSD/GPU/CXL placement.
- Tree initialization helps construction and query seed selection, but its value is dataset-dependent.
- Later graph methods may outperform EFANNA because of stronger pruning, connectivity, or routing components rather than because they abandon EFANNA's initialization idea.

## Related Pages

- [EFANNA](../entities/efanna.md)
- [NN-Descent](../entities/nn-descent.md)
- [FLANN](../entities/flann.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
