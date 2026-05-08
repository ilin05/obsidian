---
id: gnns
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - method
  - ann
  - graph-index
  - routing
source_count: 1
sources:
  - raw/sources/papers/gnns-knn-graph-2011.pdf
related:
  - approximate-nearest-neighbor-search
  - proximity-graph-theory-for-ann
  - nn-descent
  - navigable-small-world-graph
  - hnsw
confidence: high
---

# GNNS

## Profile

Graph Nearest Neighbor Search (GNNS) is an early graph ANN method that searches a kNN graph by starting from sampled nodes and greedily walking toward closer neighbors.

## Why It Matters

- It makes the graph-search pattern explicit before later hierarchical and diversified graph indexes.
- It separates query-time controls such as restarts and expansions from construction-time graph quality.
- It is a useful predecessor for reading [NSW](navigable-small-world-graph.md), [HNSW](hnsw.md), and kNNG-based graph traversal papers.

## Limits

GNNS depends on a prebuilt kNN graph and uses relatively simple random-start hill climbing. Modern methods usually improve it through better seeds, hierarchy, neighbor pruning, connectivity repair, or beam-style routing.

## Related Pages

- [GNNS Source Note](../source-notes/gnns-knn-graph-2011.md)
- [NN-Descent](nn-descent.md)
- [Navigable Small World Graph](navigable-small-world-graph.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
