---
id: falconn
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - library
  - ann
  - lsh
  - angular-distance
source_count: 1
sources:
  - raw/sources/papers/falconn-lsh-angular-2015.pdf
related:
  - approximate-nearest-neighbor-search
  - ann-benchmarks
  - ann-benchmarking-methodology
  - flann
confidence: high
---

# FALCONN

## Profile

FALCONN is a practical LSH library for angular distance based on cross-polytope LSH. It combines asymptotically strong LSH theory with implementation techniques such as fast pseudo-random rotations, the Fast Hadamard Transform, feature hashing, and multiprobe querying.

## Why It Matters

- It keeps the ANN knowledge base from becoming graph-only.
- It is an important angular/cosine-distance baseline family for ANN-Benchmarks-style evaluation.
- It helps separate metric assumptions: cosine/angular workloads may need different baselines than L2 graph comparisons.

## Limits

FALCONN is not a graph index and should not be used to explain graph-pruning behavior. Its primary scope is angular/cosine LSH; other distances or MIPS settings need separate support.

## Related Pages

- [FALCONN Source Note](../source-notes/falconn-lsh-angular-2015.md)
- [ANN-Benchmarks](ann-benchmarks.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
- [FLANN](flann.md)
