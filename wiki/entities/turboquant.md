---
id: turboquant
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - method
  - vector-quantization
  - turboquant
  - kv-cache
  - retrieval
source_count: 1
sources:
  - raw/sources/papers/turboquant-2025.pdf
related:
  - turboquant-2025
  - qjl
  - product-quantization
  - vector-quantization
  - kv-cache-quantization
  - approximate-nearest-neighbor-search
confidence: high
---

# TurboQuant

## Profile

TurboQuant is a general-purpose online vector quantization method aimed at both low MSE reconstruction and low-distortion unbiased inner-product estimation.

## Core Mechanism

- Randomly rotate the input vector so coordinates become easier to quantize independently.
- Apply near-optimal scalar quantization per coordinate to minimize MSE.
- Quantize the residual with a 1-bit QJL stage to recover unbiased inner-product estimation.

## Why It Is Important

- It connects theory, LLM inference, and vector retrieval in one framework.
- It explicitly tries to beat the offline-learning tradeoffs of [Product Quantization](product-quantization.md).
- It turns [QJL](qjl.md) from a standalone KV-cache method into a reusable second-stage building block.

## Caveats

- The practical cost of the rotation and residual pipeline still matters for real deployment.
- Its strongest claims depend on how well the theoretical constants translate to production workloads.

## Related Pages

- [TurboQuant Source Note](../source-notes/turboquant-2025.md)
- [QJL](qjl.md)
- [Product Quantization](product-quantization.md)
