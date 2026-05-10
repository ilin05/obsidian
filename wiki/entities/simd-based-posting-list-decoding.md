---
id: simd-based-posting-list-decoding
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - paper
  - simd
  - posting-lists
  - integer-compression
source_count: 1
sources:
  - raw/sources/papers/simd-posting-list-decoding-2011.pdf
related:
  - simd-and-vectorization-for-ann-systems
  - stream-vbyte
  - simd-compression-and-intersection
confidence: high
---

# SIMD-Based Posting List Decoding

## Profile

SIMD-Based Decoding of Posting Lists studies how compressed integer posting-list formats can be decoded with SIMD shuffle instructions. It is a search-systems precursor for fast ID-list handling in vector databases.

## Core Lesson

The compression format must be designed for SIMD decoding. Branch-heavy variable-byte formats do not automatically become fast unless control and data layout expose fixed-width batch operations.

## Related Pages

- [SIMD-Based Decoding Source Note](../source-notes/simd-based-decoding-posting-lists-2011.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
