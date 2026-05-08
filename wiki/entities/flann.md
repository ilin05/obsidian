---
id: flann
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - library
  - ann
  - tree-index
  - benchmarking
source_count: 1
sources:
  - raw/sources/papers/flann-2014.pdf
related:
  - approximate-nearest-neighbor-search
  - ann-benchmarks
  - ann-benchmarking-methodology
  - efanna
  - falconn
confidence: high
---

# FLANN

## Profile

FLANN is the Fast Library for Approximate Nearest Neighbors. It provides practical nearest-neighbor methods such as randomized KD forests and priority search k-means trees, plus automatic algorithm selection and parameter tuning.

## Why It Matters

- It is a canonical pre-HNSW non-graph baseline for in-memory ANN.
- It appears in benchmark suites and older computer-vision retrieval pipelines.
- Its tree-based structures help contextualize hybrid methods such as [EFANNA](efanna.md).
- It is useful when an evaluation needs a classical library baseline rather than only graph indexes.

## Limits

FLANN predates many modern graph and quantization methods. Benchmark claims should name the implementation, configuration procedure, and dataset because automatic tuning can change results substantially.

## Related Pages

- [FLANN Source Note](../source-notes/flann-2014.md)
- [ANN-Benchmarks](ann-benchmarks.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
- [EFANNA](efanna.md)
