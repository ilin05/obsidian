---
id: xnc
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - llm
  - embedding-layer
  - lossless
  - hardware-compression
source_count: 1
sources:
  - raw/sources/papers/xnc-2025.pdf
related:
  - vector-compression
confidence: medium
---

# XNC (XOR and NOT-Based Lossless Compression)

Hardware-friendly lossless compression for FP16 embedding layers in small quantized LLMs (Lee et al., ISCAS 2025).

## Core Idea

Exploit the observation that specific bit positions (2nd, 14th, 15th, 16th) in FP16 embedding weights are zero >99% of the time. XOR adjacent values to increase zeros, then NOT-transform to create contiguous patterns, enabling 9/12-bit truncation. 4 cycles compress, 3 cycles decompress.

## Key Properties

- Compression ratio: 1.34× average on embedding layers
- 4-bit quantized LLM size reduction: ~10%
- Extremely fast: 4/3 cycles for compress/decompress
- Only applicable to FP16 embedding layers in LLMs

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [XNC Source Note](../source-notes/xnc-2025.md)
