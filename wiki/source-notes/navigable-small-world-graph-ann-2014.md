---
id: navigable-small-world-graph-ann-2014
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - small-world
  - proximity-graph
source_count: 1
sources:
  - raw/sources/papers/navigable-small-world-graph-ann-2014.pdf
related:
  - navigable-small-world-graph
  - hnsw
  - proximity-graph-theory-for-ann
confidence: high
---

# Approximate Nearest Neighbor Algorithm Based on Navigable Small World Graphs

## Summary

Malkov et al. propose an ANN structure based on a navigable small-world graph in a general metric space. The graph stores data objects as vertices, uses greedy graph search, and obtains long-range navigability by retaining older approximate Delaunay links created during incremental construction.

## Key Contributions

- Frames ANN search as greedy routing over a graph that approximates Delaunay-neighbor behavior without needing exact Delaunay construction.
- Handles insertion like a query: search for approximate neighbors of the inserted point, connect to them, and preserve earlier links as long-range shortcuts.
- Allows query-time accuracy to be adjusted without rebuilding the graph.
- Discusses parallel/distributed operation because search and insertion mainly need local graph information.

## Reported Results

- Reports logarithmic-style insertion and search scaling at fixed accuracy in Euclidean simulations.
- On CoPhIR-scale experiments, reports evaluating only a tiny fraction of a 10M-object dataset for very high recall.
- Reports high query throughput in a Java implementation at lower recall settings.

## Relation To HNSW

This is the direct NSW predecessor to [HNSW](../entities/hnsw.md). HNSW keeps the navigable-small-world premise but adds an explicit hierarchy, random level assignment, and stronger neighbor-selection heuristics for high-recall behavior.

## Limits And Open Questions

- The paper predates the modern HNSW/NSG/DiskANN comparison setting.
- Its Delaunay approximation and long-range link retention are intuitive but not a complete theory of high-dimensional ANN graph quality.
- The distributed angle is suggestive, but later memory-tier and disaggregated-memory systems require more explicit data-placement analysis.

## Related Pages

- [Navigable Small World Graph](../entities/navigable-small-world-graph.md)
- [HNSW](../entities/hnsw.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
