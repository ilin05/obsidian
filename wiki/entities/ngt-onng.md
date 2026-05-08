---
id: ngt-onng
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - library
  - ann
  - graph-index
  - proximity-search
source_count: 1
sources:
  - raw/sources/papers/ngt-onng-2018.pdf
related:
  - approximate-nearest-neighbor-search
  - ann-benchmarking-methodology
  - fanng
  - flann
  - proximity-graph-theory-for-ann
confidence: high
---

# NGT / ONNG

## Profile

NGT is a graph-based proximity-search library lineage from Yahoo Japan. The ONNG optimization paper refines kNN-graph indexing through degree adjustment, path adjustment, and traversal implementation improvements.

## Why It Matters

- It represents a practical graph-library branch alongside HNSW and NSG.
- It focuses on indegree, outdegree, and redundant paths, which makes it a useful engineering complement to RNG/MRNG pruning theory.
- It uses tree-based seed acquisition before graph exploration, linking it to tree-plus-graph hybrids such as EFANNA.

## Limits

This page is currently grounded in the ONNG optimization paper only. The original NGT/PANNG lineage and library documentation may need separate source coverage if NGT becomes a major baseline in future work.

## Related Pages

- [NGT / ONNG Source Note](../source-notes/ngt-onng-2018.md)
- [FANNG](fanng.md)
- [FLANN](flann.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
