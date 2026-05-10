---
id: segpq
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - product-quantization
  - lossless
  - codebook
source_count: 1
sources:
  - raw/sources/papers/segpq-2025.pdf
related:
  - vector-compression
  - product-quantization
  - deltapq
confidence: high
---

# SegPQ

Lossless PQ codebook compression via learned piecewise linear approximation (Liu et al., PVLDB 2025).

## Core Idea

Fit an error-bounded piecewise linear approximation (ε-PLA) model to the sorted PQ codebook (codewords concatenated into integer keys). Store compact line segments + low-bit residuals instead of raw codewords. Compatible with any PQ variant.

## Key Properties

- Codebook compression: up to 4.7× (~851 MB for 1B vectors)
- Query overhead: only 3.3% (SIMD-aware processing)
- Theoretical bound: 1.721 + ⌈log₂ ε_OPT⌉ bits per codeword
- Validated in real RAG pipeline (Llama 3.2 + Wikipedia)

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [SegPQ Source Note](../source-notes/segpq-2025.md)
- [Product Quantization](product-quantization.md)
- [DeltaPQ](deltapq.md)
