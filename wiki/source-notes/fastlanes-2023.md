---
id: fastlanes-2023
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - simd
  - vectorization
  - compression
  - database-systems
  - columnar-storage
source_count: 1
sources:
  - raw/sources/papers/fastlanes-2023.pdf
related:
  - fastlanes
  - product-quantization
  - simd-and-vectorization-for-ann-systems
  - milvus
  - faiss
confidence: high
---

# The FastLanes Compression Layout: Decoding > 100 Billion Integers per Second with Scalar Code

## Bibliographic Note

Azim Afroozeh and Peter A. Boncz. 2023. *The FastLanes Compression Layout: Decoding > 100 Billion Integers per Second with Scalar Code*. PVLDB 16(9), 2132-2144. DOI: `10.14778/3598581.3598587`.

Project/artifacts: <https://github.com/cwida/FastLanes>.

The related entity page is [FastLanes](../entities/fastlanes.md).

## Core Problem

FastLanes targets data-parallel decompression under heterogeneous SIMD hardware. Existing layouts often depend on a specific SIMD width or require hand-written intrinsics. That creates maintenance cost and prevents newer/wider hardware from being exploited automatically.

The paper redesigns the compression layout so decoding exposes independent work to scalar code, compiler auto-vectorization, and multiple SIMD widths.

## Design

FastLanes is built around a virtual 1024-bit SIMD model.

- **FLMM1024 virtual register:** design layouts as if decoding targets a 1024-bit register, then map to scalar code or existing SIMD ISAs.
- **1024-bit interleaved bit-packing:** expose more independent bit-unpacking work than conventional horizontal layouts.
- **Unified Transposed Layout:** reorder values so DELTA decoding has SIMD-friendly independent lanes across 8-, 16-, 32-, and 64-bit types.
- **Scalar implementation:** use simple scalar operations that modern compilers can auto-vectorize.
- **Vector-at-a-time decoding:** decode compressed chunks into 1024-value arrays used directly by a vectorized query pipeline.

## Evaluation Facts

- The paper reports decoding over 100 billion integers per second.
- It reports decoding over 40 values per CPU cycle in some cases.
- End-to-end query experiments integrated FastLanes into an experimental Tectorwise vectorized query processor.
- On RAM-resident query pipelines, FastLanes can improve performance by reducing memory bandwidth pressure even after accounting for decompression.
- The paper reports up to 7x end-to-end improvement versus uncompressed data and up to 4x versus scalar compression in the tested query setting.

## Relevance To ANNS Systems

FastLanes matters for ANN systems because vector search frequently uses compressed representations: PQ codes, scalar quantization, posting lists, vector IDs, and attribute filters.

The key lesson is that compressed representation and execution kernel must be co-designed. A compact format that is not SIMD/vectorization friendly may lose its benefit during scan, filter, or rerank. Conversely, a layout that exposes independent work can reduce bandwidth while keeping CPU pipelines busy.

## Follow-up Questions

- Could PQ/SQ code layouts borrow FastLanes-style interleaving to improve ADC and rerank scans?
- How should an ANN system balance SIMD-friendly layout against random access during graph traversal?
- Can FastLanes-style scalar auto-vectorization reduce architecture-specific code paths in vector databases?
