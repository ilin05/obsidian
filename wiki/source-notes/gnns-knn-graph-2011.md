---
id: gnns-knn-graph-2011
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - knn-graph
  - routing
source_count: 1
sources:
  - raw/sources/papers/gnns-knn-graph-2011.pdf
related:
  - gnns
  - nn-descent
  - navigable-small-world-graph
  - approximate-nearest-neighbor-search
confidence: high
---

# Fast Approximate Nearest-Neighbor Search with k-Nearest Neighbor Graph

## Summary

Hajebi, Abbasi-Yadkori, Shahbazi, and Zhang propose Graph Nearest Neighbor Search (GNNS), an early graph ANN method that builds a kNN graph offline and answers queries through repeated local hill-climbing from sampled starting nodes.

## Key Contributions

- Uses a k-nearest-neighbor graph as the search structure rather than only a tree or hash table.
- Searches by selecting random entry points, greedily moving to closer graph neighbors, and keeping candidate expansions bounded by search parameters.
- Separates construction-time graph quality from query-time controls such as number of random restarts, greedy steps, and neighbor expansions.
- Provides an early empirical comparison against KD-tree and LSH-style baselines for image descriptors.

## Why It Matters

GNNS is a direct ancestor of later graph traversal methods. It lacks the hierarchy and stronger neighbor-selection rules of [HNSW](../entities/hnsw.md), [NSG](../entities/nsg.md), or Vamana, but it makes the basic local-search pattern explicit: start somewhere, walk through a neighborhood graph, and hope the graph gives a descent path toward the query.

## Limits And Open Questions

- Random entry points can waste work if the graph lacks useful long links or good seeds.
- The method depends on having a kNN graph already built; it does not solve scalable graph construction by itself.
- It is most useful as historical and component-level context rather than as a modern baseline.

## Related Pages

- [GNNS](../entities/gnns.md)
- [NN-Descent](../entities/nn-descent.md)
- [Navigable Small World Graph](../entities/navigable-small-world-graph.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
