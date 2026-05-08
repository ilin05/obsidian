---
id: falconn-lsh-angular-2015
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - lsh
  - angular-distance
  - benchmarking
source_count: 1
sources:
  - raw/sources/papers/falconn-lsh-angular-2015.pdf
related:
  - falconn
  - ann-benchmarks
  - ann-benchmarking-methodology
  - flann
confidence: high
---

# Practical and Optimal LSH for Angular Distance

## Summary

Andoni, Indyk, Laarhoven, Razenshteyn, and Schmidt present a practical cross-polytope locality-sensitive hashing family for angular distance. The method matches the asymptotically optimal LSH exponent for angular distance while being practical enough to outperform hyperplane LSH in experiments.

## Key Contributions

- Analyzes cross-polytope LSH for angular/cosine distance on the unit sphere.
- Provides a fine-grained lower bound for angular-distance LSH quality tradeoffs.
- Introduces a multiprobe scheme for cross-polytope LSH to reduce memory overhead.
- Uses fast pseudo-random rotations via the Fast Hadamard Transform and feature hashing to make the method practical.
- Reports substantially faster query time than hyperplane LSH and linear scan on datasets in the 100K to 100M range.

## Why It Matters

FALCONN is the main practical LSH counterweight to the graph-heavy ANN branch. It is valuable for [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md) because cosine/angular workloads should not be explained only through HNSW/NSG-style graph indexes.

## Limits And Open Questions

- The method targets angular distance; Euclidean or MIPS claims require care.
- LSH's theoretical guarantees do not remove the need for implementation-level benchmarking, especially against modern graph and quantization methods.
- FALCONN is useful as a baseline family, but many contemporary production systems prefer graph or quantization hybrids for high recall.

## Related Pages

- [FALCONN](../entities/falconn.md)
- [ANN-Benchmarks](../entities/ann-benchmarks.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
- [FLANN](../entities/flann.md)
