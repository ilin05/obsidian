---
id: quantization-to-speedup-ann-2024
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - product-quantization
  - ivf
  - hnsw
source_count: 1
sources:
  - raw/sources/papers/quantization-to-speedup-ann-2024.pdf
related:
  - quantization-to-speedup-ann
  - scalar-and-binary-quantization-for-ann
  - approximate-nearest-neighbor-search
  - hnsw
confidence: medium
---

# Quantization to Speedup Approximate Nearest Neighbor Search

## Summary

This paper targets IVF-PQ style ANN search and argues that a fixed `nprobe` is wasteful because different queries need different numbers of coarse clusters. It uses an IVF-HNSW framework with learned early termination and per-centroid HNSW structures for large clusters.

## Key Mechanism

- Uses HNSW to search coarse centroids in an IVF-PQ style pipeline.
- Learns an adaptive termination condition to predict query-specific `nprobe`.
- Builds small HNSW structures for dense centroids so searching within large clusters avoids scanning too many entries.

## Reported Results

- The paper reports speedups over IVF-HNSW while preserving recall on tested datasets.
- It reports that many queries require much smaller `nprobe` values than a uniform conservative setting.

## Why It Matters

The paper is useful as a supporting source for adaptive termination in quantization-based ANN. It is less about scalar quantization itself and more about reducing wasted work around quantized IVF partitions.

## Limits And Open Questions

- The paper is lower priority than PQ Fast Scan, Quicker ADC, RaBitQ extension, and Low-Precision Quantization for KNN.
- Its contribution is mainly adaptive search control around IVF-HNSW rather than a new quantization primitive.

## Related Pages

- [Quantization to Speedup ANN](../entities/quantization-to-speedup-ann.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
