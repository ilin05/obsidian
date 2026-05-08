---
id: vector-quantization-research-overview
type: overview
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - overview
  - vector-quantization
  - kv-cache
  - retrieval
source_count: 3
sources:
  - raw/QJL.pdf
  - raw/pq.pdf
  - raw/TurboQuant.pdf
related:
  - vector-quantization
  - kv-cache-quantization
  - approximate-nearest-neighbor-search
  - qjl
  - product-quantization
  - turboquant
confidence: medium
---

# Vector Quantization Research Overview

## Current Cluster

This vault now has an initial cluster around vector quantization for two adjacent use cases:

- compressing KV caches for LLM inference
- compressing vectors for approximate nearest-neighbor retrieval

## Current Synthesis

- [Product Quantization](../entities/product-quantization.md) is the classical retrieval baseline: split vectors into subspaces, quantize each separately, and combine short codes with ADC and IVFADC for scalable ANN search.
- [RaBitQ](../entities/rabitq.md) adds a more theory-driven ANN path: randomized, data-oblivious quantization with strong inner-product error guarantees, extended here to multi-bit codes and practical high-recall search.
- [QJL](../entities/qjl.md) is the most specialized source in the cluster: it targets KV-cache quantization, preserves inner products via a quantized JL sketch, and argues that removing quantization metadata overhead is a major win.
- [TurboQuant](../entities/turboquant.md) is the most ambitious synthesis source: it aims for near-optimal distortion rates, uses rotation plus scalar quantization for MSE, and then applies QJL to the residual for unbiased inner products.

## Emerging Thesis

The cluster suggests a movement from offline codebook-heavy quantization toward lighter online methods that preserve the geometry needed by the downstream task:

- retrieval wants compact searchable codes with strong recall
- attention wants inner-product fidelity with minimal runtime and metadata overhead
- newer methods try either to specialize harder for retrieval efficiency or to unify retrieval and inference under one geometric quantization story

## Open Questions

- When does a tuned offline PQ system still beat online methods strongly enough to justify the extra indexing complexity?
- Where does RaBitQ sit empirically versus TurboQuant on ANN workloads once both are engineered well?
- Does TurboQuant beat plain QJL on KV-cache tasks at equal latency and memory budgets, or is the best choice task-specific?
- How much of the practical win in modern systems comes from better distortion and how much comes from lower metadata and indexing overhead?

## Entry Points

- [Vector Quantization](../topics/vector-quantization.md)
- [KV Cache Quantization](../topics/kv-cache-quantization.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
