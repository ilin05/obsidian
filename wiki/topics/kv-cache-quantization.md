---
id: kv-cache-quantization
type: topic
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - kv-cache
  - llm-inference
  - quantization
source_count: 2
sources:
  - raw/sources/papers/qjl-2024.pdf
  - raw/sources/papers/turboquant-2025.pdf
related:
  - qjl
  - turboquant
  - vector-quantization
confidence: medium
---

# KV Cache Quantization

## Summary

The current sources treat KV-cache quantization as a geometry-preservation problem under tight memory and latency constraints. The critical quantity is not only reconstruction MSE, but whether the compressed cache still supports accurate inner products for attention.

## Current View

- [QJL](../entities/qjl.md) is the sharper specialized method: quantized JL sketching plus sign bits, with a strong emphasis on removing per-block metadata overhead.
- [TurboQuant](../entities/turboquant.md) is broader: first minimize MSE with a rotation-based quantizer, then use QJL on the residual to recover unbiased inner products.
- Both sources frame long-context serving as memory-bound and latency-sensitive, so metadata and implementation overhead matter almost as much as nominal bit-width.

## Main Practical Themes

- Inner-product preservation is central because attention scores depend directly on query-key geometry.
- Outliers in deeper layers matter and can dominate distortion if left untreated.
- Reported wins are substantial at low bit-widths, but the exact best method likely depends on latency budget, hardware, and context length.

## Open Questions

- When is plain QJL enough, and when does the extra MSE stage of TurboQuant pay for itself?
- How stable are the reported low-bit "quality neutral" regimes across models and long-context benchmarks?

## Related Pages

- [QJL](../entities/qjl.md)
- [TurboQuant](../entities/turboquant.md)
- [QJL Source Note](../source-notes/qjl-2024.md)
- [TurboQuant Source Note](../source-notes/turboquant-2025.md)
