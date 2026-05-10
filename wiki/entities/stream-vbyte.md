---
id: stream-vbyte
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - codec
  - simd
  - integer-compression
source_count: 1
sources:
  - raw/sources/papers/stream-vbyte-2017.pdf
related:
  - simd-compression-and-intersection
  - simd-based-posting-list-decoding
  - simd-and-vectorization-for-ann-systems
confidence: high
---

# Stream VByte

## Profile

Stream VByte is a SIMD-friendly byte-oriented integer compression format. Its defining idea is separating control bytes from data bytes so decoding can use SIMD shuffles efficiently.

## Why It Is Important

It is a concrete layout lesson for ANN system metadata: ID lists, adjacency lists, and posting lists should be encoded in a way that lets the decoder operate branch-light and batch-oriented.

## Related Pages

- [Stream VByte Source Note](../source-notes/stream-vbyte-2017.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
