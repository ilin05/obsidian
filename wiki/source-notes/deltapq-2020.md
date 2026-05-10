---
id: deltapq-2020
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - product-quantization
  - lossless
  - code-compression
source_count: 1
sources:
  - raw/sources/papers/deltapq-2020.pdf
related:
  - vector-compression
  - product-quantization
  - segpq-2025
confidence: high
---

# DeltaPQ: Lossless Product Quantization Code Compression for High Dimensional Similarity Search

Runhui Wang and Dong Deng, PVLDB 2020 (Rutgers University).

## Summary

DeltaPQ losslessly compresses product quantization codes by storing differences between codes in a tree structure (DeltaTree) rather than the original codes themselves. The key insight: PQ codes within a dataset often have many coordinates in common, so storing per-coordinate differences `⟨i, x⟩` (log m + log l bits) is cheaper than storing the full code (m log l bits). The optimal DeltaTree is derived from the MST of a complete graph over all codes, constructed in O(2^m n) time. Queries are processed directly on compressed data via PQScan.

## Key Contributions

- **Differential tree compression for PQ codes**: Encodes a collection of PQ codes in a DeltaTree where root stores the full code and every other node stores differences from its parent.
- **Optimal DeltaTree via MST**: Proves the optimal DeltaTree (minimizing total differences) is the MST of a complete graph with Hamming-like distance between codes. Develops an O(2^m n) time, O(n) space algorithm to find it — critical since naive MST takes O(n²).
- **Direct search on compressed data**: ANNS queries can be processed directly on the compressed DeltaTree representation with only O(m) space overhead per query, supporting inner product, cosine, Euclidean, and Lp-norm.
- **Compression ratio of 2–5× on real datasets**: Across five large-scale datasets (SIFT1B, GIST1M, etc.), DeltaPQ achieves compression ratios often >2×, up to 5×, while general-purpose lossless compressors barely work on PQ codes.

## Algorithm Details

### DeltaTree Construction
1. Build a complete graph where each vertex is a PQ code, and edge weight = number of differing coordinates between two codes.
2. Compute the MST using a specialized O(2^m n) algorithm that exploits m being small (typically 8).
3. Root the MST arbitrarily; the resulting rooted tree is the DeltaTree.
4. Store: root code in full + each non-root node as a set of `⟨coordinate_index, new_value⟩` differences.

### Search on Compressed Data
- Traverse the DeltaTree from root; reconstruct each code on-the-fly by applying differences.
- PQScan computes asymmetric distances from reconstructed codes to the query using a pre-computed lookup table.

## Reported Results

- Compression ratio: 2–5× on PQ codes (SIFT1B, GIST1M, MNIST, CIFAR-10, YouTube-8M)
- General compressors (gzip, bzip2, lz4) achieve <1.1× on the same codes
- Query slowdown vs uncompressed PQScan: ~10–20%
- DeltaTree construction: linear in n, practical for billion-scale

## Significance

DeltaPQ is the first work to systematically exploit redundancies in PQ codes for lossless compression. It establishes that PQ codes — despite appearing as high-entropy integer tuples — contain significant inter-code similarities that general-purpose compressors miss. The MST-based tree construction is a clean theoretical formulation that later code compression methods build upon.

## Limits And Open Questions

- Only compresses PQ codes, not the codebook. SegPQ (2025) later addresses codebook compression.
- m must be small (≤8–10) for the O(2^m n) algorithm to be practical.
- The tree structure adds sequential dependency during decoding — random access to individual codes requires traversal from root.
- Does not exploit the fact that some dimensions may be more "stable" than others across the dataset.

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [SegPQ (2025)](segpq-2025.md)
- [Product Quantization](../entities/product-quantization.md)
