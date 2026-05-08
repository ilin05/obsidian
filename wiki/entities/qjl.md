---
id: qjl
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - method
  - kv-cache
  - qjl
  - llm-inference
source_count: 1
sources:
  - raw/QJL.pdf
related:
  - qjl-2024
  - turboquant
  - kv-cache-quantization
  - vector-quantization
confidence: high
---

# QJL

## Profile

QJL is a quantization method for preserving attention-relevant inner products in KV caches. It combines a Johnson-Lindenstrauss projection with sign-bit quantization and avoids storing per-block quantization constants.

## Core Mechanism

- Apply a random Gaussian projection to the key vector.
- Quantize the projected key to sign bits.
- Project the query with the same transform but do not quantize it.
- Estimate inner products asymmetrically from the projected query and quantized key.

## Why It Is Important

- It attacks one of the nastiest parts of low-bit KV quantization: metadata overhead.
- It treats inner-product fidelity as the core objective, which is closer to how attention actually uses cached keys.
- It is a building block inside [TurboQuant](turboquant.md), not only a standalone method.

## Caveats

- It is more specialized than a general vector quantization framework.
- Practical performance still depends on handling model outliers well.

## Related Pages

- [QJL Source Note](../source-notes/qjl-2024.md)
- [TurboQuant](turboquant.md)
- [KV Cache Quantization](../topics/kv-cache-quantization.md)
