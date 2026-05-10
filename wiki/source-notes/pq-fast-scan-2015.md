---
id: pq-fast-scan-2015
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - product-quantization
  - simd
  - fastscan
source_count: 1
sources:
  - raw/sources/papers/pq-fast-scan-2015.pdf
related:
  - pq-fast-scan
  - product-quantization
  - simd-and-vectorization-for-ann-systems
  - approximate-nearest-neighbor-search
confidence: high
---

# Cache Locality is Not Enough: High-Performance Nearest Neighbor Search with Product Quantization Fast Scan

## Summary

This paper introduces PQ Fast Scan, a CPU SIMD implementation strategy for product-quantized ANN scan. The central claim is that making PQ lookup tables fit in L1 cache is not sufficient; high performance requires moving the hot lookup operation into SIMD registers and redesigning the memory layout around in-register lookup.

## Key Mechanism

- Standard PQ Scan computes ADC distances from cache-resident lookup tables, but each candidate still requires many table accesses and does not map well to SIMD.
- PQ Fast Scan groups vectors, transposes code layout, builds small minimum tables, quantizes floating-point partial distances to 8-bit integers, and uses SIMD shuffle instructions for in-register lookup.
- The small in-register tables give lower bounds on candidate distances, pruning most accesses to the original distance tables.
- The method preserves the same result as the underlying PQ scan, so the contribution is execution efficiency rather than a new quantizer.

## Reported Results

- The paper reports 4x-6x speedup over PQ Scan on SIFT1B.
- It reports that the scan can check over a billion candidates per second on one core.
- It emphasizes that L1 miss rate alone does not explain the bottleneck: instruction shape, lookup layout, and SIMD utilization matter.

## Why It Matters

PQ Fast Scan is the key source for understanding why ANN SIMD work is not just "vectorize distance arithmetic." The memory access pattern of ADC lookup tables determines whether SIMD helps. This paper is also the conceptual predecessor for later FastScan, Quick ADC, Quicker ADC, and graph-plus-quantization systems such as SymphonyQG.

## Limits And Open Questions

- The technique is specialized to scan-style PQ/ADC workloads.
- It trades additional layout constraints and 8-bit distance quantization for speed.
- It does not by itself solve irregular graph traversal; later systems combine FastScan with graph layouts.

## Related Pages

- [PQ Fast Scan](../entities/pq-fast-scan.md)
- [Product Quantization](../entities/product-quantization.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
