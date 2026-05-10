---
id: vector-quantization
type: topic
status: active
created: 2026-04-08
updated: 2026-05-10
tags:
  - vector-quantization
  - compression
  - geometry
source_count: 13
sources:
  - raw/sources/papers/qjl-2024.pdf
  - raw/sources/papers/product-quantization-2011.pdf
  - raw/sources/papers/turboquant-2025.pdf
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
  - qjl
  - product-quantization
  - turboquant
  - rabitq
  - norm-explicit-quantization
  - scalar-and-binary-quantization-for-ann
  - simd-and-vectorization-for-ann-systems
  - kv-cache-quantization
  - approximate-nearest-neighbor-search
confidence: medium
---

# Vector Quantization

## Summary

Vector quantization is the common language across the current source cluster. The shared problem is compressing high-dimensional vectors while preserving the geometric quantity that the downstream system actually uses, such as Euclidean distance or inner products.

## Current Cluster

- [Product Quantization](../entities/product-quantization.md) is the classical retrieval-oriented answer.
- [RaBitQ](../entities/rabitq.md) is a theory-driven retrieval-oriented family built around randomized quantization and strong inner-product error guarantees.
- [QJL](../entities/qjl.md) is an inner-product-preserving sketch for KV caches with zero metadata overhead as a core design goal.
- [TurboQuant](../entities/turboquant.md) tries to unify low-MSE and unbiased inner-product quantization in one online framework.
- [Scalar and Binary Quantization for ANN](scalar-and-binary-quantization-for-ann.md) tracks per-dimension low-precision, binary, and HNSW-oriented quantization.
- [Norm-Explicit Quantization](../entities/norm-explicit-quantization.md) highlights that MIPS needs norm-aware quantization rather than plain MSE minimization.
- [PQ Fast Scan](../entities/pq-fast-scan.md), [Quicker ADC](../entities/quicker-adc.md), and [SymphonyQG](../entities/symphonyqg.md) show that quantization quality and execution layout must be co-designed.

## Working Synthesis

- Older methods are often stronger on mature indexed retrieval pipelines.
- Retrieval now has several distinct branches in this vault: learned codebook methods like PQ, randomized/data-oblivious methods like RaBitQ, scalar/low-precision methods, and graph-integrated quantized search.
- Newer methods focus on online operation, lower metadata overhead, and tighter distortion guarantees.
- The downstream task determines the right distortion target: MSE alone is often not enough.
- Execution matters: a quantizer that looks good on error can still be slow if its distance estimator requires random memory access or poor SIMD utilization.

## Open Questions

- Which distortion target best predicts downstream quality in real systems: MSE, inner-product error, or something task-specific?
- How much of quantization quality comes from geometry-preserving transforms versus learned codebooks and indexing tricks?
- Should scalar quantization be treated as a baseline storage format, a traversal-time distance estimator, or a full replacement for raw-vector reranking?
- When does explicit norm storage or quantization become mandatory for high-quality MIPS?

## Related Pages

- [Vector Quantization Research Overview](../overview/vector-quantization-research-overview.md)
- [Product Quantization](../entities/product-quantization.md)
- [RaBitQ](../entities/rabitq.md)
- [QJL](../entities/qjl.md)
- [TurboQuant](../entities/turboquant.md)
- [Scalar and Binary Quantization for ANN](scalar-and-binary-quantization-for-ann.md)
- [SIMD and Vectorization for ANN Systems](simd-and-vectorization-for-ann-systems.md)
