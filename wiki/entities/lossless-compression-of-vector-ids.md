---
id: lossless-compression-of-vector-ids
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - lossless
  - id-compression
  - ans
  - entropy-coding
source_count: 1
sources:
  - raw/sources/papers/lossless-compression-of-vector-ids-for-anns-2025.pdf
related:
  - vector-compression
  - approximate-nearest-neighbor-search
confidence: high
---

# Lossless Compression of Vector IDs

ANS-based lossless compression of auxiliary data (vector IDs, graph edges) in ANN indexes (Severo et al., Meta FAIR, arXiv 2025).

## Core Idea

Within IVF lists or graph adjacency lists, the ordering of IDs is semantically irrelevant — this frees log n! bits exploitable via set compression. Using ANS (Asymmetric Numeral Systems) and wavelet trees, compress vector IDs by up to 7× and graph edges by 2–3×. Total index size reduced by ~30% with zero accuracy or latency impact.

## Key Properties

- ID compression: up to 7× in IVF lists (32-bit → ~4.6 bits per ID)
- Total index reduction: ~30% on billion-scale
- Online-decodable: no measurable search slowdown for IVF
- Also enables entropy coding of sub-optimally quantized PQ codes

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [Source Note](../source-notes/lossless-compression-of-vector-ids-for-anns-2025.md)
