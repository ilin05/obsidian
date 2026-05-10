---
id: vector-compression
type: topic
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - product-quantization
  - lossless
  - lossy
  - storage-efficiency
source_count: 5
sources:
  - raw/sources/papers/deltapq-2020.pdf
  - raw/sources/papers/segpq-2025.pdf
  - raw/sources/papers/lep-2024.pdf
  - raw/sources/papers/lossless-compression-of-vector-ids-for-anns-2025.pdf
  - raw/sources/papers/leann-2025.pdf
related:
  - approximate-nearest-neighbor-search
  - lossless-floating-point-compression
  - product-quantization
confidence: medium
---

# Vector Compression

## Core Idea

Vector compression encompasses techniques that reduce the storage footprint of vector search systems — not just the vectors themselves, but also the auxiliary data structures (codebooks, PQ codes, vector IDs, graph edges, and entire indexes). Unlike lossless float compression for time series (Gorilla → DeXOR lineage), which targets one-dimensional streaming float streams, vector compression operates in the context of high-dimensional ANN search where the compression target may be lossy (vectors), lossless but domain-specific (PQ codes, codebooks, IDs), or structural (graph pruning, embedding recomputation).

The unifying question: given an ANN index that must fit within a storage budget, how to compress each component — vectors, quantized codes, codebooks, IDs, and graph metadata — while preserving search accuracy and latency?

## Compression Targets in an ANN Index

```
ANN Index Storage Breakdown:
├── Raw vectors (or quantized codes)  ← DeltaPQ, LEP
├── Codebook / quantization metadata  ← SegPQ
├── Vector IDs (IVF lists, graph edges) ← Lossless ID Compression
└── Graph/index structure             ← LEANN, Lossless ID Compression
```

## Techniques by Target

### PQ Code Compression — DeltaPQ (2020)

Lossless compression of the per-vector PQ codes (m integers in [1, l] each) by storing differences between similar codes in a tree. The DeltaTree is the MST of a complete graph where edge weight = number of differing coordinates. Achieves 2–5× compression on PQ codes; general-purpose compressors achieve <1.1×. Search runs directly on compressed data with O(m) overhead per query.

### Codebook Compression — SegPQ (2025)

Lossless compression of the PQ codebook (m × l centroids) by fitting an error-bounded piecewise linear approximation (ε-PLA) to sorted codeword integer keys. Achieves up to 4.7× codebook compression (~851 MB for 1B vectors) with only 3.3% query overhead. Compatible with all PQ variants. SIMD-aware query processing.

### Raw Vector Compression — LEP (2024)

Lossy compression of vector embeddings by adapting ALP's decimal-aware float encoding per-dimension. Column-wise (vertical) layout improves compression; bitmap encoding handles frequently repeated values; inter-dimension correlation exploited for delta encoding. Achieves compression ratios comparable to PQ/SQ at matched recall with lower MSE. Bridges the lossless float compression and vector quantization literatures.

### Auxiliary Data Compression — Lossless Compression of Vector IDs (2025)

Lossless compression of vector IDs in IVF lists and graph adjacency lists using ANS (Asymmetric Numeral Systems). Key insight: order of IDs within a list is semantically irrelevant, freeing log n! bits. ID compression up to 7× (32-bit → ~4.6 bits per ID), graph edges 2–3×. Total index size reduced by ~30% with zero search impact.

### Index Structure Compression — LEANN (2025)

Radical storage reduction via two ideas: (1) recompute embeddings on-the-fly at query time rather than storing them; (2) aggressively prune edges of low-degree graph nodes while preserving hub connectivity. Two-level search: cheap PQ pruning → GPU-batched encoder inference for exact reranking. Index up to 50× smaller; particularly suited to RAG where generation dominates latency.

## Design Dimensions

| Technique | Target | Lossy/Lossless | Approach | Compression | Search Overhead |
|-----------|--------|----------------|----------|-------------|-----------------|
| DeltaPQ | PQ codes | Lossless | MST differential encoding | 2–5× | ~15% |
| SegPQ | PQ codebook | Lossless | ε-PLA + residuals | up to 4.7× | ~3.3% |
| LEP | Raw vectors | Lossy | ALP per-dimension | comparable to PQ | varying |
| ID Compression | Vector IDs / edges | Lossless | ANS + wavelet trees | 2–7× | negligible |
| LEANN | Entire index | Lossy (struct.) | Recompute + graph prune | up to 50× | ~10% end-to-end |

## Cross-Cutting Themes

- **Compression stack decomposition**: The most effective systems compress different index components with different techniques — DeltaPQ for codes + SegPQ for codebooks + ID compression for listing metadata create multiplicative savings.
- **Lossless vs lossy boundary**: DeltaPQ, SegPQ, and ID compression are lossless within their domains (PQ codes, codebooks, IDs), while LEP and LEANN are lossy at the vector/value level. The boundary is determined by whether the compressed data directly feeds distance computation.
- **Set/order irrelevance as a compression primitive**: Both DeltaPQ (MST over codes) and ID compression (set encoding) exploit the fact that proximity in the original space matters more than ordering — a principle that may generalize to other index components.
- **Columnar data layout for vectors**: LEP's finding that vertical (per-dimension) layout improves compression echoes database column-store principles and suggests co-design between compression and storage layout.
- **Recompute vs store**: LEANN's radical approach — eliminate storage by recomputing — represents an endpoint on the compression spectrum. It works when computation is cheaper than storage and when encoder access is guaranteed.

## Open Questions

- Can DeltaPQ (code compression) and SegPQ (codebook compression) be combined for multiplicative savings at the cost of additive overhead?
- Does vertical (columnar) data layout — shown beneficial in LEP — improve other compression schemes beyond ALP-based encoding?
- What is the information-theoretic lower bound on lossless PQ code compression given the codebook?
- Can LEANN's embedding recomputation approach be made encoder-agnostic (e.g., via cached intermediate representations)?
- How do these techniques compose with hardware constraints (SIMD width, GPU batch size, SSD access granularity)?
- Is there a unified compression budget allocation framework that optimally distributes bits across vectors, codes, codebooks, and metadata?

## Related Pages

- [DeltaPQ (2020)](../source-notes/deltapq-2020.md) · [DeltaPQ Entity](../entities/deltapq.md)
- [SegPQ (2025)](../source-notes/segpq-2025.md) · [SegPQ Entity](../entities/segpq.md)
- [LEP (2024)](../source-notes/lep-2024.md) · [LEP Entity](../entities/lep.md)
- [Lossless Compression of Vector IDs (2025)](../source-notes/lossless-compression-of-vector-ids-for-anns-2025.md)
- [LEANN (2025)](../source-notes/leann-2025.md) · [LEANN Entity](../entities/leann.md)
- [XNC (2025)](../source-notes/xnc-2025.md) · [XNC Entity](../entities/xnc.md)
- [Approximate Nearest Neighbor Search](approximate-nearest-neighbor-search.md)
- [Lossless Floating Point Compression](lossless-floating-point-compression.md)
- [Product Quantization](../entities/product-quantization.md)
