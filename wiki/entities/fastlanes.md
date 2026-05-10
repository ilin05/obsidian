---
id: fastlanes
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - simd
  - vectorization
  - compression
  - columnar-storage
source_count: 1
sources:
  - raw/sources/papers/fastlanes-2023.pdf
related:
  - simd-and-vectorization-for-ann-systems
  - product-quantization
  - milvus
  - faiss
confidence: high
---

# FastLanes

## Profile

FastLanes is a PVLDB 2023 compression-layout project for extremely fast integer decoding. Its core idea is to design the compressed layout around a virtual 1024-bit SIMD model while keeping the implementation portable and scalar-code friendly.

## Main Ideas

- Use a 1024-bit interleaved bit-packing layout.
- Use a Unified Transposed Layout to expose independent DELTA decoding work across lane widths.
- Write scalar code that compilers can auto-vectorize.
- Decode compressed chunks directly into vectorized execution pipelines.

## Position In The Vault

FastLanes is a layout-and-execution reference for ANN systems that store compressed vector codes, vector IDs, posting lists, or attribute data. It reinforces that compression format design should account for decode and scan kernels, not only size.

## Related Pages

- [Source Note](../source-notes/fastlanes-2023.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
- [Product Quantization](product-quantization.md)
- [Milvus](milvus.md)
- [FAISS](faiss.md)
