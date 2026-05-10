---
id: hnsw-lavq
type: entity
status: seed
created: 2026-05-10
updated: 2026-05-10
tags:
  - method
  - ann
  - hnsw
  - scalar-quantization
source_count: 1
sources:
  - raw/sources/papers/quantization-enhanced-hnsw-lavq-2025.pdf
related:
  - hnsw
  - scalar-and-binary-quantization-for-ann
confidence: low
---

# HNSW-LAVQ

## Profile

HNSW-LAVQ is an anonymous OpenReview scalar-quantized HNSW proposal. It uses percentile clipping to reduce outlier damage in int8 scalar quantization and AVX2 kernels for integer distance computation.

## Caveat

Use as a low-confidence design idea, not as a mature baseline, until the paper is accepted or independently reproduced.

## Related Pages

- [HNSW-LAVQ Source Note](../source-notes/quantization-enhanced-hnsw-lavq-2025.md)
- [Scalar and Binary Quantization for ANN](../topics/scalar-and-binary-quantization-for-ann.md)
