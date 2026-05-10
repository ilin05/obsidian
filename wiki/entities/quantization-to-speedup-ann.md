---
id: quantization-to-speedup-ann
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - paper
  - ann
  - ivf
  - hnsw
source_count: 1
sources:
  - raw/sources/papers/quantization-to-speedup-ann-2024.pdf
related:
  - approximate-nearest-neighbor-search
  - hnsw
  - product-quantization
confidence: medium
---

# Quantization To Speedup ANN

## Profile

This paper proposes an IVF-HNSW acceleration strategy for quantization-based ANN. It focuses on adaptive `nprobe` and per-centroid HNSW structures rather than a new scalar or vector quantizer.

## Core Lesson

Uniform probe counts waste work. Query-specific termination and better handling of large centroid buckets can reduce IVF-PQ query cost.

## Related Pages

- [Quantization to Speedup ANN Source Note](../source-notes/quantization-to-speedup-ann-2024.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
