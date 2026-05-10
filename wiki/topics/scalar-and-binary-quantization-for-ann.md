---
id: scalar-and-binary-quantization-for-ann
type: topic
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - scalar-quantization
  - binary-quantization
  - hnsw
  - vector-search
source_count: 10
sources:
  - raw/sources/papers/faiss-library-2025.pdf
  - raw/sources/papers/scann-2020.pdf
  - raw/sources/papers/rabitq-extension-2024.pdf
  - raw/sources/papers/low-precision-quantization-knn-2021.pdf
  - raw/sources/papers/norm-explicit-quantization-mips-2020.pdf
  - raw/sources/papers/quantization-to-speedup-ann-2024.pdf
  - raw/sources/papers/symphonyqg-2024.pdf
  - raw/sources/papers/aqr-hnsw-2026.pdf
  - raw/sources/papers/quantization-enhanced-hnsw-lavq-2025.pdf
  - raw/sources/papers/information-theoretic-binarization-vector-search-2026.pdf
related:
  - vector-quantization
  - approximate-nearest-neighbor-search
  - simd-and-vectorization-for-ann-systems
  - faiss
  - scann
  - rabitq
  - low-precision-quantization-knn
  - norm-explicit-quantization
  - symphonyqg
  - aqr-hnsw
  - hnsw-lavq
confidence: medium
---

# Scalar And Binary Quantization For ANN

## Summary

This topic tracks scalar, binary, and low-precision quantization for ANN systems. The common goal is to reduce vector memory and distance-computation cost without destroying the ordering needed by nearest-neighbor search.

## Current View

**Scalar quantization as a systems baseline:** [FAISS](../entities/faiss.md) gives the mature library context for SQ8-style indexes and refinement patterns. Scalar quantization is attractive because it keeps per-dimension arithmetic simple and does not require PQ-style table lookup.

**Distance-preserving low precision:** [Low-Precision Quantization for KNN](../entities/low-precision-quantization-knn.md) frames int8 quantization around preserving nearest-neighbor distance ordering rather than reconstructing vectors. This is a useful implementation-level view for existing KNN indexes.

**Inner-product sensitivity:** [Norm-Explicit Quantization](../entities/norm-explicit-quantization.md) shows that MIPS is sensitive to vector norm error. This is the main reason MSE or Euclidean reconstruction error can be a misleading design objective for inner-product retrieval.

**Theory-driven bit quantization:** [RaBitQ](../entities/rabitq.md) and its multi-bit extension provide a data-oblivious alternative to PQ/SQ with explicit error-bound claims and a pruning strategy based on the most significant bits.

**Graph plus quantization:** [SymphonyQG](../entities/symphonyqg.md) shows a stronger integration path: quantized codes must be placed next to graph vertices, graph degree should align with FastScan batch size, and explicit reranking can reintroduce random-memory costs.

**Scalar-quantized HNSW branch:** [AQR-HNSW](../entities/aqr-hnsw.md) and [HNSW-LAVQ](../entities/hnsw-lavq.md) are recent low-confidence sources on percentile/density-aware scalar quantization for HNSW. They are useful design probes but should not be treated as established baselines yet.

**Binary/extreme compression branch:** [Information-Theoretic Binarization for Vector Search](../entities/information-theoretic-binarization-vector-search.md) is a speculative source on binary representation as an architecture-level change rather than only a compression setting.

## Mechanism-Level Distinctions

- **SQ/int8:** quantize each dimension independently. Fast and hardware-friendly, but range selection and outliers matter.
- **Binary/1-bit:** extreme compression and bitwise distance, but usually needs careful oversampling/reranking or a new retrieval architecture.
- **Norm-explicit:** store or quantize vector norm separately when the downstream score is inner product.
- **RaBitQ-style:** randomized/data-oblivious quantization with explicit estimator guarantees and bitwise/SIMD-friendly distance estimation.
- **Graph-integrated quantization:** quantization affects not only stored vectors but also graph traversal, candidate ordering, reranking, and memory-access locality.

## Research Implications

Scalar quantization should be treated as a full execution path, not merely a storage format. A useful system design must specify:

- where full-precision vectors are kept, if anywhere
- whether quantized distances guide traversal, filtering, reranking, or final scoring
- how outliers/range bounds are chosen
- whether query vectors are quantized globally, per segment, or per graph neighborhood
- whether recall is recovered through oversampling, reranking, better quantization, or graph co-design

## Open Questions

- Should scalar-quantized HNSW store raw vectors for final reranking, or rely on stronger quantized distances?
- For graph ANN, is quantization most valuable for traversal decisions, final reranking, or memory footprint?
- When does explicit norm storage outperform ordinary scalar quantization for inner-product embeddings?
- Can binary quantization become a primary retrieval representation, or is it mostly a first-stage candidate generator?

## Related Pages

- [Vector Quantization](vector-quantization.md)
- [Approximate Nearest Neighbor Search](approximate-nearest-neighbor-search.md)
- [SIMD and Vectorization for ANN Systems](simd-and-vectorization-for-ann-systems.md)
