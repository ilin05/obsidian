---
id: low-precision-quantization-knn-2021
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - scalar-quantization
  - low-precision
  - knn
source_count: 1
sources:
  - raw/sources/papers/low-precision-quantization-knn-2021.pdf
related:
  - low-precision-quantization-knn
  - scalar-and-binary-quantization-for-ann
  - vector-quantization
  - approximate-nearest-neighbor-search
confidence: medium
---

# Low-Precision Quantization for Efficient Nearest Neighbor Search

## Summary

This paper proposes an implementation-level low-precision quantization approach for KNN search. Instead of learning a codebook, it maps vectors and distance computations into low-precision integer domains while trying to preserve nearest-neighbor distance ordering.

## Key Mechanism

- Uses feature-wise/data-driven quantization ranges.
- Replaces full-precision vectors with compact integer representations such as int8.
- Quantizes the distance function as well as the stored vectors, so query-time distance computation can use cheaper integer operations.
- Frames quantization quality around preserving distance order rather than minimizing reconstruction error.

## Reported Results

- Reports around 60% memory reduction and about 30% query-throughput improvement with roughly 2% recall reduction in the primary HNSWlib setting.
- Reports comparable memory/runtime improvements on FAISS exact search and NGT, with modest recall loss.

## Why It Matters

This is a useful bridge between textbook scalar quantization and production implementation. It treats quantization as a drop-in distance-kernel/storage optimization that can combine with existing KNN indexes.

## Limits And Open Questions

- The evidence is implementation-focused and narrower than system-level billion-scale studies.
- The method depends on dataset value distributions and may require careful range selection.

## Related Pages

- [Low-Precision Quantization for KNN](../entities/low-precision-quantization-knn.md)
- [Scalar and Binary Quantization for ANN](../topics/scalar-and-binary-quantization-for-ann.md)
