---
id: quicker-adc-2019
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - product-quantization
  - simd
  - faiss
source_count: 1
sources:
  - raw/sources/papers/quicker-adc-2019.pdf
related:
  - quicker-adc
  - pq-fast-scan
  - product-quantization
  - faiss
  - simd-and-vectorization-for-ann-systems
confidence: high
---

# Quicker ADC: Unlocking the Hidden Potential of Product Quantization with SIMD

## Summary

Quicker ADC generalizes the Quick ADC/PQ Fast Scan line of work to broader product quantizer configurations and newer SIMD instructions. It targets the ADC distance-computation bottleneck in PQ-based ANN systems and evaluates the method inside FAISS with inverted multi-index and IVF-HNSW settings.

## Key Mechanism

- Uses SIMD in-register lookup and saturated arithmetic to speed ADC.
- Introduces irregular product quantizers that combine subquantizers of different bit widths, such as 6/6/4-bit groupings.
- Uses split tables to support lookup tables larger than a SIMD register, including 8-bit subquantizers through 6-bit or 7-bit shuffles.
- Integrates into FAISS, making the technique testable against optimized PQ, polysemous codes, and multiple index types.

## Reported Results

- The paper reports that Quicker ADC outperforms FAISS and polysemous-code baselines in many SIFT1000M and Deep1B configurations.
- It reports especially strong gains for R@100 and for combinations with IVF-HNSW.
- It argues that Quicker ADC can bring PQ distance evaluation close to binary-code speed while preserving stronger PQ recall behavior.

## Why It Matters

This is a direct bridge between ANN algorithms and hardware-conscious implementation. It shows that PQ code design, lookup-table width, SIMD shuffle availability, and index structure must be co-designed.

## Limits And Open Questions

- The benefits depend on the target SIMD ISA, especially AVX2/AVX-512 capabilities.
- The paper is primarily about distance evaluation, not full vector database system behavior.
- Some projected benefits rely on instruction support that was not fully available at the time of evaluation.

## Related Pages

- [Quicker ADC](../entities/quicker-adc.md)
- [PQ Fast Scan](../entities/pq-fast-scan.md)
- [FAISS](../entities/faiss.md)
