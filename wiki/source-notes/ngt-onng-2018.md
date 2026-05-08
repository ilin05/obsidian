---
id: ngt-onng-2018
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - proximity-search
  - pruning
source_count: 1
sources:
  - raw/sources/papers/ngt-onng-2018.pdf
related:
  - ngt-onng
  - nn-descent
  - fanng
  - flann
  - ann-benchmarking-methodology
confidence: high
---

# Optimization of Indexing Based on k-Nearest Neighbor Graph for Proximity Search in High-dimensional Data

## Summary

Iwasaki and Miyazaki optimize graph-based proximity search in NGT/ONNG by focusing on indegree, outdegree, path redundancy, and implementation overhead during graph traversal. The paper treats graph performance as a degree-and-path management problem rather than only a nearest-neighbor quality problem.

## Key Contributions

- Proposes static degree adjustment over both outgoing and incoming edges derived from a KNN graph.
- Adds constrained static degree adjustment to avoid nodes with excessive outdegree.
- Proposes dynamic degree adjustment so query-time expansion can depend on the required search accuracy.
- Introduces path adjustment to remove shortcut edges that have short alternative paths.
- Improves visited-node management during graph exploration, which can be a large part of query time.
- Uses NGT's tree-based seed acquisition, based on VP-tree-style entry selection, before graph exploration.

## Why It Matters

NGT/ONNG is a practical graph-library branch that complements [HNSW](../entities/hnsw.md) and [NSG](../entities/nsg.md). Its emphasis on indegree, outdegree, and alternative paths is useful when reading RNG-style pruning papers: the final graph should preserve reachability while suppressing edges that add search work.

## Limits And Open Questions

- This note covers the ONNG optimization paper, not every NGT/PANNG source in the NGT lineage.
- The paper focuses on in-memory graph search and does not evaluate modern SSD/GPU/CXL settings.
- Because it is tightly tied to NGT implementation choices, component-level comparison is safer than treating ONNG as only an abstract graph.

## Related Pages

- [NGT / ONNG](../entities/ngt-onng.md)
- [FANNG](../entities/fanng.md)
- [FLANN](../entities/flann.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
