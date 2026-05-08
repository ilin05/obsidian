---
id: product-quantization-for-nearest-neighbor-search-2011
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - vector-quantization
  - retrieval
  - ann
  - product-quantization
source_count: 1
sources:
  - raw/sources/papers/product-quantization-2011.pdf
related:
  - product-quantization
  - approximate-nearest-neighbor-search
  - vector-quantization
confidence: high
---

# Product Quantization for Nearest Neighbor Search

## Summary

This paper is a foundational retrieval paper on compressing database vectors with product quantization. The method splits a vector into low-dimensional subspaces, quantizes each subspace separately, and stores the resulting compact code. Distances are then approximated from lookup tables rather than exact full-vector comparisons.

## Key Contributions

- Introduces product quantization (PQ) as a practical way to obtain short codes with much lower memory use than storing full vectors.
- Emphasizes asymmetric distance computation (ADC): database vectors are coded, but the query is not, which improves precision.
- Combines PQ with inverted files (IVFADC) for large-scale non-exhaustive search.
- Shows scalability to very large datasets, including a cited experiment at the scale of 2 billion vectors.

## Main Ideas To Keep

- PQ is retrieval-first: its goal is fast approximate nearest-neighbor search under tight memory budgets.
- ADC is a key pattern because it keeps the query in original space and reduces quantization error at query time.
- IVFADC is the systems pattern that makes PQ scalable in practice by narrowing search to a coarse partition before scanning PQ codes.

## Why It Matters

- This is the baseline method that later retrieval-focused quantization papers still argue against or extend.
- It establishes the recurring tension between code quality, memory cost, and indexing/search efficiency.
- In the current cluster, it is the clearest contrast to [TurboQuant](../entities/turboquant.md): PQ is codebook-based and offline, while TurboQuant aims to be online and data-oblivious.

## Limits And Open Questions

- The method depends on learned codebooks and an indexing pipeline, so it is not ideal for fast online quantization scenarios such as KV-cache updates.
- This source predates later optimized PQ variants and modern GPU-oriented implementations.
- It remains an open practical question how far online methods can close the gap with tuned PQ systems on mature ANN workloads.

## Related Pages

- [Product Quantization](../entities/product-quantization.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Vector Quantization](../topics/vector-quantization.md)
