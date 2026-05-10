---
id: simd-compression-and-intersection
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - paper
  - simd
  - integer-compression
  - set-intersection
source_count: 1
sources:
  - raw/sources/papers/simd-compression-intersection-2014.pdf
related:
  - simd-based-posting-list-decoding
  - stream-vbyte
  - simd-and-vectorization-for-ann-systems
confidence: high
---

# SIMD Compression and Intersection

## Profile

This paper studies SIMD compression and intersection for sorted integer lists. It matters for vector search when ANN is combined with inverted lists, graph/list IDs, or metadata filters.

## Core Lesson

Fast decompression is only useful if the next operator is also fast. The paper therefore vectorizes both integer decoding and list intersection.

## Related Pages

- [SIMD Compression Source Note](../source-notes/simd-compression-intersection-2014.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
