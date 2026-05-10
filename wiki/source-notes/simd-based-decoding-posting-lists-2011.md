---
id: simd-based-decoding-posting-lists-2011
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - information-retrieval
  - simd
  - integer-compression
  - posting-lists
source_count: 1
sources:
  - raw/sources/papers/simd-posting-list-decoding-2011.pdf
related:
  - simd-based-posting-list-decoding
  - simd-and-vectorization-for-ann-systems
confidence: high
---

# SIMD-Based Decoding of Posting Lists

## Summary

This paper studies SIMD acceleration for decoding compressed posting lists. Its core relevance to ANNS is not vector distance computation directly, but the compressed-ID/list-processing layer used by inverted indexes, filters, and hybrid vector-text systems.

## Key Mechanism

- Builds a taxonomy of byte-oriented integer encodings and identifies formats amenable to SIMD decoding.
- Uses SIMD shuffle instructions such as PSHUFB to decode groups of variable-length integers with few branches.
- Presents new group encodings and generic SIMD algorithms for decoding them.

## Reported Results

- The paper reports SIMD decoding up to 3x faster than non-SIMD decoding.
- It also notes that the compression format matters: not every variable-byte format becomes fast merely by adding SIMD.

## Why It Matters

Vector databases increasingly combine vector ANN with inverted-list metadata filters, segment IDs, neighbor IDs, or compressed posting lists. This paper explains how list format and SIMD decoding must be co-designed.

## Limits And Open Questions

- The source targets search-engine posting lists, not ANN vector payloads.
- It should be used as a systems-design analogue for ID lists and filters, not as evidence for distance-kernel acceleration.

## Related Pages

- [SIMD-Based Posting List Decoding](../entities/simd-based-posting-list-decoding.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
