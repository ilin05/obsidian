---
id: relative-nn-descent-2023
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - construction
  - rng
source_count: 1
sources:
  - raw/sources/papers/relative-nn-descent-2023.pdf
related:
  - relative-nn-descent
  - nn-descent
  - nsg
  - hnsw
  - proximity-graph-theory-for-ann
confidence: high
---

# Relative NN-Descent: A Fast Index Construction for Graph-Based Approximate Nearest Neighbor Search

## Summary

Ono and Matsui propose Relative NN-Descent (RNN-Descent), a graph-index construction algorithm that combines NN-Descent with RNG Strategy. The goal is to build a high-quality graph index directly and faster, without first constructing a full approximate KNN graph and without using ANN search during insertion.

## Key Contributions

- Identifies graph construction time as a major bottleneck for graph-based ANN indexes.
- Combines NN-Descent-style neighbor-of-neighbor updates with RNG Strategy-style edge removal.
- Avoids the two-step "build KNN graph then refine" pipeline used by refinement-based methods such as NSG.
- Avoids direct HNSW-style insertion that depends on running ANN search inside the partially built index.
- Preserves connectivity through the neighborhood update design.

## Reported Results

- Reports construction roughly twice as fast as NSG on GIST1M while maintaining comparable search performance.
- Reports faster construction than NN-Descent itself in the tested setting.
- Includes SIFT20M experiments to test larger construction behavior.

## Importance

This paper is less about a new search traversal rule and more about the cost of building the graph that traversal needs. It is useful when comparing graph ANN methods under dynamic or frequently rebuilt workloads where construction time matters as much as recall-latency curves.

The original [NN-Descent](nn-descent-2011.md) paper is now ingested, so claims about RNN-Descent's construction changes can be read against the underlying neighbor-propagation primitive.

## Limits And Open Questions

- The main contribution is construction speed, not a new memory-tier design.
- The paper's scale is useful but not a full billion-scale serving study.
- Some datasets can produce high out-degree vertices, which can slow search unless additional degree control is applied.

## Related Pages

- [Relative NN-Descent](../entities/relative-nn-descent.md)
- [NN-Descent](../entities/nn-descent.md)
- [NSG](../entities/nsg.md)
- [HNSW](../entities/hnsw.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
