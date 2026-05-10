---
id: symphonyqg
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - method
  - ann
  - graph-index
  - quantization
  - simd
source_count: 1
sources:
  - raw/sources/papers/symphonyqg-2024.pdf
related:
  - rabitq
  - hnsw
  - simd-and-vectorization-for-ann-systems
  - scalar-and-binary-quantization-for-ann
confidence: medium
---

# SymphonyQG

## Profile

SymphonyQG is a graph ANN method that integrates RaBitQ-style quantization and FastScan-style SIMD distance estimation into graph traversal and graph construction.

## Core Mechanism

- Store neighbor quantization codes next to graph vertices for sequential access.
- Use FastScan to estimate neighbor distances in batches.
- Avoid explicit final reranking to reduce random raw-vector accesses.
- Refine graph degree so each vertex aligns with FastScan batch size.

## Why It Is Important

SymphonyQG is a concrete example of graph/quantization co-design. It shows that simply adding quantized distance estimates to HNSW-like traversal is not enough; layout and graph degree must match the execution kernel.

## Related Pages

- [SymphonyQG Source Note](../source-notes/symphonyqg-2024.md)
- [RaBitQ](rabitq.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
