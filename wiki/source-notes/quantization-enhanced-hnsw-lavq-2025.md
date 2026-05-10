---
id: quantization-enhanced-hnsw-lavq-2025
type: source-note
status: seed
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - hnsw
  - scalar-quantization
  - avx2
source_count: 1
sources:
  - raw/sources/papers/quantization-enhanced-hnsw-lavq-2025.pdf
related:
  - hnsw-lavq
  - scalar-and-binary-quantization-for-ann
  - hnsw
confidence: low
---

# Quantization-Enhanced HNSW for Scalable Approximate Vector Search

## Summary

This anonymous OpenReview paper proposes HNSW-LAVQ, a scalar-quantized HNSW variant that uses percentile clipping to avoid outlier-dominated min-max quantization and an AVX2 integer L2 kernel for faster search.

## Key Mechanism

- Computes per-dimension percentile bounds such as 1st and 99th percentile.
- Clips outliers before mapping values to int8, preserving resolution in dense regions of the distribution.
- Replaces float32 distance computation with AVX2 integer distance computation.
- Keeps the HNSW graph algorithm mostly unchanged while changing the storage and distance kernel.

## Reported Results

- Reports about 3.8x memory reduction and 4.4x QPS improvement over float32 HNSW on SIFT1M.
- Reports Recall@1 around 97% in the highlighted SIFT1M setting.

## Why It Matters

This source gives a clear, intuition-friendly scalar quantization mechanism for HNSW: naive min-max SQ can waste bins on outliers, while percentile clipping can preserve useful resolution for the bulk of vectors.

## Limits And Open Questions

- Confidence is low because the paper is anonymous and under review.
- The benchmark scope is narrow; use it as a design idea, not a strong literature pillar.
- The method needs comparison against mature FAISS/Lucene-style scalar quantizers and reranking strategies.

## Related Pages

- [HNSW-LAVQ](../entities/hnsw-lavq.md)
- [Scalar and Binary Quantization for ANN](../topics/scalar-and-binary-quantization-for-ann.md)
