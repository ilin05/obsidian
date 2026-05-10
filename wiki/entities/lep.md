---
id: lep
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - lossy
  - float-encoding
  - embedding
source_count: 1
sources:
  - raw/sources/papers/lep-2024.pdf
related:
  - vector-compression
  - lossless-floating-point-compression
  - alp-2023
confidence: medium
---

# LEP (Lossily Encoded floating-Points)

Lossy variant of ALP for compressing vector embeddings (Krippner, Master's Thesis 2024, TU Munich / CWI).

## Core Idea

Apply ALP's decimal-aware float encoding dimension-by-dimension to embedding matrices. By truncating low bits, LEP becomes a tunable lossy compressor achieving compression ratios comparable to PQ/SQ with lower reconstruction error (MSE). Also exploits per-dimension data layouts, frequent-value bitmap compression, and inter-dimension correlation.

## Key Properties

- Bridges lossless float compression (ALP lineage) with vector quantization
- Tunable lossy compression: fewer bits → higher compression, lower accuracy
- Vertical (columnar) data layout outperforms horizontal for compression
- Not peer-reviewed at a top venue (Master's thesis)

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [LEP Source Note](../source-notes/lep-2024.md)
- [Lossless Floating Point Compression](../topics/lossless-floating-point-compression.md)
- [ALP (2023)](../source-notes/alp-2023.md)
