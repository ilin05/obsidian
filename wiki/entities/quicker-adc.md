---
id: quicker-adc
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
  - raw/sources/papers/quicker-adc-2019.pdf
related:
  - pq-fast-scan
  - product-quantization
  - faiss
  - simd-and-vectorization-for-ann-systems
confidence: high
---

# Quicker ADC

## Profile

Quicker ADC is a SIMD implementation family for asymmetric distance computation over product-quantized codes. It generalizes earlier Quick ADC/PQ Fast Scan work to broader PQ code widths and newer SIMD instructions.

## Core Mechanism

- Use in-register lookup for ADC partial distances.
- Support irregular product quantizers with mixed subquantizer widths.
- Use split lookup tables to emulate larger lookup tables with available SIMD shuffles.
- Integrate the implementation into FAISS and evaluate with IMI, IVF, and IVF-HNSW.

## Why It Is Important

Quicker ADC is a direct example of hardware-aware quantizer design: the best PQ configuration is not just the one with low distortion, but the one whose lookup table and code layout match SIMD instructions.

## Related Pages

- [Quicker ADC Source Note](../source-notes/quicker-adc-2019.md)
- [PQ Fast Scan](pq-fast-scan.md)
- [FAISS](faiss.md)
