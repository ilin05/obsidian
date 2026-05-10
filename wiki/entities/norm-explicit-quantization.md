---
id: norm-explicit-quantization
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - method
  - mips
  - vector-quantization
  - inner-product
source_count: 1
sources:
  - raw/sources/papers/norm-explicit-quantization-mips-2020.pdf
related:
  - vector-quantization
  - product-quantization
  - scalar-and-binary-quantization-for-ann
confidence: high
---

# Norm-Explicit Quantization

## Profile

Norm-Explicit Quantization (NEQ) is a paradigm for maximum inner product search that quantizes vector norm and direction separately. It can wrap existing VQ families such as PQ, OPQ, RQ, and AQ.

## Core Lesson

For MIPS, norm error can dominate inner-product error. A lower Euclidean reconstruction error is therefore not necessarily the right objective for inner-product retrieval.

## Related Pages

- [NEQ Source Note](../source-notes/norm-explicit-quantization-mips-2020.md)
- [Vector Quantization](../topics/vector-quantization.md)
