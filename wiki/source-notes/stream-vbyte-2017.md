---
id: stream-vbyte-2017
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - integer-compression
  - simd
  - search-systems
source_count: 1
sources:
  - raw/sources/papers/stream-vbyte-2017.pdf
related:
  - stream-vbyte
  - simd-compression-and-intersection
  - simd-and-vectorization-for-ann-systems
confidence: high
---

# Stream VByte: Faster Byte-Oriented Integer Compression

## Summary

Stream VByte is a byte-oriented integer compression format designed for fast SIMD decoding. It separates the control stream from encoded data so the decoder can use simple SIMD shuffles with fewer branch and dependency costs.

## Key Mechanism

- Keeps VByte-like engineering simplicity but separates control bytes and data bytes.
- Decodes groups of integers by loading control, using a shuffle table, and applying SIMD byte shuffle to the data stream.
- Works well for delta-coded sorted integer sequences, such as document IDs or row IDs.

## Reported Results

- The paper reports decoding up to twice as fast as VARINT-G8IU on real datasets.
- It reports more than 4 billion differentially coded integers per second from RAM to L1 cache on a 3.4 GHz Haswell processor.

## Why It Matters

This is a practical codec source for ANN systems that need compressed IDs, posting lists, graph adjacency lists, or segment metadata. The important lesson is that physical encoding should expose SIMD-friendly control flow.

## Limits And Open Questions

- The codec optimizes integer streams, not floating-point or quantized vector payloads.
- Whether it helps an ANN system depends on whether ID/list processing is on the critical path.

## Related Pages

- [Stream VByte](../entities/stream-vbyte.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
