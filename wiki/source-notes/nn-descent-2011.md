---
id: nn-descent-2011
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - knn-graph
  - construction
source_count: 1
sources:
  - raw/sources/papers/nn-descent-2011.pdf
related:
  - nn-descent
  - approximate-nearest-neighbor-search
  - proximity-graph-theory-for-ann
  - ann-benchmarking-methodology
confidence: high
---

# Efficient K-Nearest Neighbor Graph Construction for Generic Similarity Measures

## Summary

Dong, Charikar, and Li introduce NN-Descent, an approximate k-nearest-neighbor graph construction algorithm for generic similarity measures. The core observation is that a neighbor of a neighbor is likely to be a neighbor, so the algorithm repeatedly propagates and evaluates candidate neighbors through the current graph rather than comparing all pairs.

## Key Contributions

- Frames approximate KNN graph construction as a reusable primitive for similarity search, clustering, data mining, and graph-based ANN.
- Uses local neighbor propagation to reduce distance comparisons while improving graph quality iteratively.
- Keeps only minimal per-node auxiliary state, making the method memory-light relative to more complex construction pipelines.
- Supports generic similarity measures rather than depending on Euclidean geometry or tree partitioning.
- Reports high graph recall with a small fraction of exhaustive pairwise comparisons and empirical scaling close to subquadratic behavior.

## Why It Matters

NN-Descent is the construction backbone behind KGraph-style baselines and many later graph ANN pipelines. It is especially important for [NSG](../entities/nsg.md), [DPG](../entities/diversified-proximity-graph.md), NSSG/SSG-style methods, and Relative NN-Descent, because these papers often start from an approximate KNN graph and then prune or diversify edges.

## Limits And Open Questions

- The paper optimizes KNN graph construction, not the final query traversal policy by itself.
- Later graph indexes may use the NN-Descent graph only as an intermediate candidate pool, so graph-construction recall should not be confused with search recall.
- Publication metadata should be checked carefully when citing KGraph as a library versus NN-Descent as the paper.

## Related Pages

- [NN-Descent](../entities/nn-descent.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
- [Relative NN-Descent](../entities/relative-nn-descent.md)
