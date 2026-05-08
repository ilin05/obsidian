---
id: vector-quantization
type: topic
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - vector-quantization
  - compression
  - geometry
source_count: 3
sources:
  - raw/sources/papers/qjl-2024.pdf
  - raw/sources/papers/product-quantization-2011.pdf
  - raw/sources/papers/turboquant-2025.pdf
related:
  - qjl
  - product-quantization
  - turboquant
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

## Working Synthesis

- Older methods are often stronger on mature indexed retrieval pipelines.
- Retrieval now has at least two distinct modern branches in this vault: learned codebook methods like PQ and randomized/data-oblivious methods like RaBitQ.
- Newer methods focus on online operation, lower metadata overhead, and tighter distortion guarantees.
- The downstream task determines the right distortion target: MSE alone is often not enough.

## Open Questions

- Which distortion target best predicts downstream quality in real systems: MSE, inner-product error, or something task-specific?
- How much of quantization quality comes from geometry-preserving transforms versus learned codebooks and indexing tricks?

## Related Pages

- [Vector Quantization Research Overview](../overview/vector-quantization-research-overview.md)
- [Product Quantization](../entities/product-quantization.md)
- [RaBitQ](../entities/rabitq.md)
- [QJL](../entities/qjl.md)
- [TurboQuant](../entities/turboquant.md)
