---
id: norm-explicit-quantization-mips-2020
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - mips
  - vector-quantization
  - norm
  - inner-product
source_count: 1
sources:
  - raw/sources/papers/norm-explicit-quantization-mips-2020.pdf
related:
  - norm-explicit-quantization
  - scalar-and-binary-quantization-for-ann
  - vector-quantization
  - product-quantization
confidence: high
---

# Norm-Explicit Quantization: Improving Vector Quantization for Maximum Inner Product Search

## Summary

This paper argues that quantization for maximum inner product search should not blindly minimize reconstruction/MSE error. It decomposes quantization error into norm error and direction error, then shows that norm error has a stronger effect on inner-product accuracy.

## Key Mechanism

- Separates each item into norm and direction.
- Quantizes the norm explicitly with dedicated norm codebooks.
- Uses existing VQ methods such as PQ, OPQ, RQ, or AQ for the direction component.
- Recombines approximate norm and approximate direction when estimating query-item inner products.

## Reported Results

- The paper reports that NEQ variants consistently improve PQ, OPQ, RQ, and AQ for MIPS on tested datasets.
- It reports that norm-explicit variants can improve recall-item behavior even when their ordinary Euclidean reconstruction error is not smaller.

## Why It Matters

This is directly relevant to the user's TurboQuant/QJL questions: preserving inner products is not the same as minimizing MSE. It also explains why explicitly storing or estimating norms can be valuable in quantized retrieval pipelines.

## Limits And Open Questions

- NEQ is a paradigm around VQ/codebooks rather than a complete production vector database.
- It targets MIPS specifically; Euclidean ANN can have different error sensitivities.

## Related Pages

- [Norm-Explicit Quantization](../entities/norm-explicit-quantization.md)
- [Vector Quantization](../topics/vector-quantization.md)
