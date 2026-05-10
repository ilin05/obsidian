---
id: segpq-2025
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - product-quantization
  - lossless
  - codebook-compression
  - pla
source_count: 1
sources:
  - raw/sources/papers/segpq-2025.pdf
related:
  - vector-compression
  - product-quantization
  - deltapq-2020
confidence: high
---

# Not Small Enough? SegPQ: A Learned Approach to Compress Product Quantization Codebooks

Qiyu Liu, Yanlin Qi, Siyuan Han, Jingshu Peng, Jin Li, and Lei Chen, PVLDB 2025.

## Summary

SegPQ losslessly compresses PQ codebooks (the centroid tables), which can reach several gigabytes for web-scale datasets. The key idea: project each codeword to an integer key via binary concatenation, sort the keys, then fit an error-bounded piecewise linear approximation (ε-PLA) model. The codebook is stored as compact line segments (slope + intercept + breakpoint) plus low-bit residuals. SIMD-aware query processing exploits hardware parallelism on the compressed codebook. Achieves up to 4.7× codebook compression with only 3.3% query overhead.

## Key Contributions

- **Codebook compression via learned models**: First work to apply ε-PLA to losslessly compress PQ codebooks. Theoretically bounds encoding at 1.721 + ⌈log₂ ε_OPT⌉ bits per codeword.
- **Generality**: Compatible with any PQ variant (OPQ, AQ, etc.) — acts as a drop-in post-processing step after codebook generation.
- **SIMD-aware query processing**: Decompression and distance computation are designed to exploit SIMD lanes for minimal query overhead.
- **Real-world RAG validation**: Integrated into a Llama 3.2 + Wikipedia RAG pipeline, reducing total memory from 7.25 GiB to 4.62 GiB while improving generation quality by 1.12×.

## Algorithm Details

### Compression
1. Concatenate each m-dimensional codeword into a single integer key.
2. Sort keys and fit ε-PLA model: a set of line segments (breakpoint s, slope α, intercept β) such that for each codeword at position i, |(α × i + β) − key_i| ≤ ε.
3. Store: segment table (compact) + residual array (⌈log₂(2ε)⌉ bits per entry).

### Decompression
1. For codeword at position i: find enclosing segment, compute PLA prediction, add residual.
2. Split integer key back into m subspace indices.

### Query Processing
- SIMD-vectorized residual addition and codeword reconstruction.
- Pre-computed distance lookup table reused as in standard PQ.

## Reported Results

- Codebook compression: up to 4.7× (~851 MB for 1B vectors with PQ(8,8))
- Query processing overhead: 3.3% additional time vs uncompressed PQ
- Six real-world datasets (SIFT1B, Deep1B, GIST1M, etc.)
- RAG pipeline: 7.25 GiB → 4.62 GiB total memory

## Significance

SegPQ fills a gap left by DeltaPQ: DeltaPQ compresses the PQ codes (per-vector representations), while SegPQ compresses the codebook (per-centroid representations). Together they form complementary approaches to PQ storage reduction. The use of learned indexing models (ε-PLA) for compression rather than indexing is a novel cross-domain application.

## Limits And Open Questions

- The ε-PLA model fitting is dataset-dependent; worst-case codeword distributions may yield poor fits.
- Concatenating codewords into a single integer assumes a total ordering that may not preserve all cross-subspace structure.
- Does not address PQ code compression — orthogonal to DeltaPQ; combining both could yield multiplicative savings.

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [DeltaPQ (2020)](deltapq-2020.md)
- [Product Quantization](../entities/product-quantization.md)
