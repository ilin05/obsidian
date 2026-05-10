---
id: deltapq
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - product-quantization
  - lossless
source_count: 1
sources:
  - raw/sources/papers/deltapq-2020.pdf
related:
  - vector-compression
  - product-quantization
  - segpq
confidence: high
---

# DeltaPQ

Lossless compression scheme for PQ codes (Wang & Deng, PVLDB 2020).

## Core Idea

Store differences between PQ codes in a tree (DeltaTree = MST of code similarity graph) rather than storing the full codes. Each difference is a `⟨coordinate_index, value⟩` pair taking log m + log l bits instead of m log l bits. Optimal tree found in O(2^m n) time.

## Key Properties

- Lossless compression of PQ codes: 2–5× ratio
- Direct search on compressed data via PQScan
- General-purpose compressors achieve <1.1× on the same data
- Only compresses codes, not the codebook

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [DeltaPQ Source Note](../source-notes/deltapq-2020.md)
- [Product Quantization](product-quantization.md)
- [SegPQ](segpq.md)
