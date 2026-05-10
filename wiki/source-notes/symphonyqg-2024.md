---
id: symphonyqg-2024
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - graph-index
  - quantization
  - simd
source_count: 1
sources:
  - raw/sources/papers/symphonyqg-2024.pdf
related:
  - symphonyqg
  - scalar-and-binary-quantization-for-ann
  - simd-and-vectorization-for-ann-systems
  - rabitq
  - hnsw
confidence: medium
---

# SymphonyQG: Towards Symphonious Integration of Quantization and Graph for Approximate Nearest Neighbor Search

## Summary

SymphonyQG integrates quantization and graph ANN more tightly than earlier NGT-QG-style systems. It combines RaBitQ-style codes, FastScan-style SIMD distance estimation, graph data layout, implicit reranking, and graph refinement aligned with batch computation.

## Key Mechanism

- Stores neighbor quantization codes compactly beside each graph vertex so candidate-neighbor distance estimates can be read sequentially.
- Uses RaBitQ and FastScan for batched distance estimation during graph search.
- Avoids an explicit reranking step by keeping selected exact or more accurate candidates during traversal, reducing random raw-vector accesses.
- Refines graph out-degree so neighbor batches align with FastScan's SIMD batch size.
- Uses FastScan during graph construction to speed indexing.

## Reported Results

- Reports 1.5x-4.5x QPS over the strongest tested baselines at 95% recall.
- Reports 3.5x-17x QPS over HNSWlib across tested datasets.
- Reports indexing at least 8x faster than NGT-QG, while using less memory than NGT-QG but more memory than vanilla graph baselines.

## Why It Matters

This is one of the strongest current bridges between the SIMD/PQ scan branch and the graph ANN branch. It shows that graph search can benefit from quantized SIMD distance estimation only when layout, reranking, graph degree, and indexing are co-designed.

## Limits And Open Questions

- SymphonyQG stores duplicated quantization codes for neighbors, increasing index size.
- It supports Euclidean distance and cosine similarity, aligned with the quantization method.
- The source is a 2024 arXiv/SIGMOD 2025 preprint in this vault; treat claims as medium confidence until cross-checked with published artifacts.

## Related Pages

- [SymphonyQG](../entities/symphonyqg.md)
- [RaBitQ](../entities/rabitq.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
