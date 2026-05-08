---
id: flann-2014
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - library
  - tree-index
  - benchmarking
source_count: 1
sources:
  - raw/sources/papers/flann-2014.pdf
related:
  - flann
  - ann-benchmarks
  - ann-benchmarking-methodology
  - efanna
  - falconn
confidence: high
---

# Scalable Nearest Neighbor Algorithms for High Dimensional Data

## Summary

Muja and Lowe present FLANN, a library and algorithm-selection framework for approximate nearest-neighbor matching. The paper evaluates randomized KD forests, priority search k-means trees, binary-feature matching with hierarchical clustering trees, automatic configuration, and distributed nearest-neighbor matching.

## Key Contributions

- Identifies randomized KD forests and priority search k-means trees as strong practical methods for high-dimensional visual features.
- Provides automatic algorithm selection and parameter tuning based on dataset characteristics and target precision.
- Adds binary-feature matching through multiple hierarchical clustering trees.
- Discusses distributed nearest-neighbor matching for data too large for one machine.
- Releases the work as the FLANN open-source library, later incorporated into OpenCV.

## Why It Matters

FLANN is a canonical non-graph baseline for [ANN-Benchmarks](../entities/ann-benchmarks.md). It also matters for graph methods because later systems such as [EFANNA](../entities/efanna.md) reuse tree-based structures for seed acquisition or graph construction initialization.

## Limits And Open Questions

- FLANN is strongest as a classical in-memory tree/library baseline; it is not a modern high-recall graph frontier.
- Automatic configuration is implementation-sensitive, so benchmark results should identify the exact FLANN version and tuning protocol.
- The paper predates many high-recall graph methods and modern hardware-tier systems.

## Related Pages

- [FLANN](../entities/flann.md)
- [ANN-Benchmarks](../entities/ann-benchmarks.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
- [EFANNA](../entities/efanna.md)
