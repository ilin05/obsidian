---
id: simd-compression-intersection-2014
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - information-retrieval
  - simd
  - integer-compression
  - set-intersection
source_count: 1
sources:
  - raw/sources/papers/simd-compression-intersection-2014.pdf
related:
  - simd-compression-and-intersection
  - simd-and-vectorization-for-ann-systems
confidence: high
---

# SIMD Compression and the Intersection of Sorted Integers

## Summary

This paper combines SIMD integer compression with SIMD set intersection for sorted integer lists. It is relevant to ANN systems wherever vector search is fused with inverted lists, filters, compressed graph/list IDs, or text/vector hybrid retrieval.

## Key Mechanism

- Uses SIMD bit unpacking and differential coding for sorted integer lists.
- Introduces SIMD intersection algorithms, including SIMD Galloping.
- Emphasizes that decompression speed alone is not enough: downstream list processing must also be vectorized.

## Reported Results

- Reports S4-BP128-D4 decoding as low as about 0.7 CPU cycles per decoded 32-bit integer.
- Reports SIMD intersection procedures often up to about 2x faster than strong non-SIMD baselines.
- Shows SIMD-based conjunctive-query processing can double the speed of an existing fast framework without sacrificing compression.

## Why It Matters

ANN engines often spend time outside the floating-point distance kernel: reading IDs, intersecting filter candidates, traversing compressed adjacency/list structures, or scanning inverted partitions. This paper gives a concrete model for optimizing those list paths.

## Limits And Open Questions

- It is a posting-list/search-engine source rather than an ANN source.
- Transfer to vector databases depends on whether filters and candidate lists are sorted, compressed, and processed in bulk.

## Related Pages

- [SIMD Compression and Intersection](../entities/simd-compression-and-intersection.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
