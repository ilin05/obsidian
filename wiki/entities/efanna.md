---
id: efanna
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - method
  - ann
  - graph-index
  - tree-index
source_count: 1
sources:
  - raw/sources/papers/efanna-2016.pdf
related:
  - approximate-nearest-neighbor-search
  - nn-descent
  - flann
  - nsg
  - ann-benchmarking-methodology
confidence: high
---

# EFANNA

## Profile

EFANNA is an approximate nearest-neighbor method that combines randomized tree structures with kNN graph construction and graph search. It uses tree-derived candidates to improve graph initialization and query seed quality.

## Why It Matters

- It is a bridge between tree-index methods such as [FLANN](flann.md) and graph-index methods such as [NSG](nsg.md).
- It shows that initialization quality can strongly affect graph construction cost.
- It helps interpret graph survey components: initialization, seed acquisition, and routing should be evaluated separately.

## Limits

EFANNA is an in-memory algorithm and should not be used as evidence for SSD/GPU/CXL behavior. Its contribution is mainly in construction and seeding, not in a new graph-theoretic pruning target.

## Related Pages

- [EFANNA Source Note](../source-notes/efanna-2016.md)
- [NN-Descent](nn-descent.md)
- [FLANN](flann.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
