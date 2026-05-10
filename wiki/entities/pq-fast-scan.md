---
id: pq-fast-scan
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - method
  - ann
  - product-quantization
  - simd
source_count: 1
sources:
  - raw/sources/papers/pq-fast-scan-2015.pdf
related:
  - product-quantization
  - quicker-adc
  - simd-and-vectorization-for-ann-systems
confidence: high
---

# PQ Fast Scan

## Profile

PQ Fast Scan is an execution-layer optimization for product-quantized ANN scan. It keeps the PQ/ADC search semantics but changes the layout and lookup path so partial-distance lookup can happen inside SIMD registers.

## Core Mechanism

- Transpose PQ codes for SIMD-friendly batch access.
- Build small quantized lookup tables that fit in SIMD registers.
- Use SIMD shuffle instructions instead of cache-resident random table lookup.
- Use lower-bound filtering to avoid many full lookup-table accesses.

## Why It Is Important

It shows that PQ performance is controlled by memory-access shape, not only by compression ratio or recall. Later ANN SIMD work, including Quicker ADC and FastScan-based graph systems, builds on this idea.

## Related Pages

- [PQ Fast Scan Source Note](../source-notes/pq-fast-scan-2015.md)
- [Product Quantization](product-quantization.md)
