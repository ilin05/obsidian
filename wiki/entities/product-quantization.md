---
id: product-quantization
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - method
  - ann
  - retrieval
  - product-quantization
source_count: 1
sources:
  - raw/sources/papers/product-quantization-2011.pdf
related:
  - product-quantization-for-nearest-neighbor-search-2011
  - approximate-nearest-neighbor-search
  - vector-quantization
  - turboquant
confidence: high
---

# Product Quantization

## Profile

Product Quantization (PQ) is a retrieval-oriented vector compression method that decomposes vectors into subspaces, quantizes each subspace independently, and uses the resulting short codes for approximate nearest-neighbor search.

## Core Mechanism

- Split a vector into low-dimensional sub-vectors.
- Learn a subquantizer for each subspace.
- Represent each database vector by the indices of its subspace centroids.
- Estimate distances from lookup tables instead of decoding full vectors every time.

## System Patterns

- ADC keeps the query unquantized and usually improves accuracy.
- IVFADC adds a coarse partitioning stage so search does not scan the whole database.

## Why It Is Important

- PQ is the main classical baseline in this vault for memory-efficient ANN retrieval.
- Later methods still position themselves relative to PQ on recall, indexing time, and codebook overhead.

## Caveats

- It typically requires offline learning and indexing.
- It fits stable retrieval indexes better than online per-token quantization workloads.

## Related Pages

- [PQ Source Note](../source-notes/product-quantization-for-nearest-neighbor-search-2011.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [TurboQuant](turboquant.md)
