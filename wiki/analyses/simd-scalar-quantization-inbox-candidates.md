---
id: simd-scalar-quantization-inbox-candidates
type: analysis
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - simd
  - vectorization
  - scalar-quantization
  - literature-map
source_count: 14
sources:
  - raw/sources/papers/quicker-adc-2019.pdf
  - raw/sources/papers/pq-fast-scan-2015.pdf
  - raw/sources/papers/rethinking-simd-vectorization-2015.pdf
  - raw/sources/papers/simd-compression-intersection-2014.pdf
  - raw/sources/papers/stream-vbyte-2017.pdf
  - raw/sources/papers/simd-posting-list-decoding-2011.pdf
  - raw/sources/papers/rabitq-extension-2024.pdf
  - raw/sources/papers/aqr-hnsw-2026.pdf
  - raw/sources/papers/quantization-enhanced-hnsw-lavq-2025.pdf
  - raw/sources/papers/symphonyqg-2024.pdf
  - raw/sources/papers/information-theoretic-binarization-vector-search-2026.pdf
  - raw/sources/papers/quantization-to-speedup-ann-2024.pdf
  - raw/sources/papers/low-precision-quantization-knn-2021.pdf
  - raw/sources/papers/norm-explicit-quantization-mips-2020.pdf
related:
  - simd-and-vectorization-for-ann-systems
  - approximate-nearest-neighbor-search
  - vector-quantization
  - product-quantization
  - rabitq
  - faiss
confidence: medium
---

# SIMD And Scalar Quantization Ingested Candidates

## Scope

This page records the paper batch ingested from `raw/inbox/` for the ANN execution and quantization branch. The selection targets two gaps in the current vault:

- SIMD/vectorization mechanisms for ANN scan paths, especially PQ/ADC lookup-table scans, integer decoding, posting-list style filtering, and database operator vectorization.
- Scalar, binary, and low-precision quantization methods that reduce vector memory and accelerate distance computation, especially for HNSW or graph-based vector search.

Selection bias: prioritize papers that explain mechanisms useful for ANNS systems work. Preprints and under-review papers are included only when they are directly tied to scalar/bit quantization or HNSW acceleration, and should be ingested with lower confidence until independently validated.

## Ingested Queue

| Source file | Paper | Priority | Why it belongs |
|---|---|---:|---|
| `raw/sources/papers/pq-fast-scan-2015.pdf` | Fabien Andre, Anne-Marie Kermarrec, Nicolas Le Scouarnec, "Cache Locality is Not Enough: High-Performance Nearest Neighbor Search with Product Quantization Fast Scan" | 1 | Direct ANN/PQ SIMD paper. It turns cache-resident PQ lookup tables into register-sized tables and is the key bridge from PQ to in-register SIMD scan. |
| `raw/sources/papers/quicker-adc-2019.pdf` | Fabien Andre, Anne-Marie Kermarrec, Nicolas Le Scouarnec, "Quicker ADC: Unlocking the Hidden Potential of Product Quantization with SIMD" | 1 | Direct ANN/PQ SIMD paper extending Quick ADC/PQ Fast Scan ideas to broader PQ code widths and AVX-512, including FAISS and IVF-HNSW settings. |
| `raw/sources/papers/rethinking-simd-vectorization-2015.pdf` | Orestis Polychroniou, Arun Raghavan, Kenneth A. Ross, "Rethinking SIMD Vectorization for In-Memory Databases" | 2 | Not ANN-specific, but useful for SIMD operator design with gathers/scatters, selections, hash tables, partitioning, and memory-resident analytics kernels. |
| `raw/sources/papers/simd-posting-list-decoding-2011.pdf` | Alexander A. Stepanov et al., "SIMD-Based Decoding of Posting Lists" | 2 | Search-engine/posting-list SIMD decoding. Relevant to inverted-list ANN, filtered ANN, compressed ID lists, and scan-side decompression. |
| `raw/sources/papers/simd-compression-intersection-2014.pdf` | Daniel Lemire, Leonid Boytsov, Nathan Kurz, "SIMD Compression and the Intersection of Sorted Integers" | 2 | Explains SIMD integer compression plus SIMD intersection. Useful for IVF posting lists, metadata filters, and compressed graph/list IDs. |
| `raw/sources/papers/stream-vbyte-2017.pdf` | Daniel Lemire, Nathan Kurz, Christoph Rupp, "Stream VByte: Faster Byte-Oriented Integer Compression" | 2 | Practical SIMD-friendly integer codec with separated control/data streams; relevant to compressed IDs and vector database segment layouts. |
| `raw/sources/papers/rabitq-extension-2024.pdf` | Jianyang Gao et al., "Practical and Asymptotically Optimal Quantization of High-Dimensional Vectors in Euclidean Space for Approximate Nearest Neighbor Search" | 1 | Multi-bit extension of RaBitQ. Important for the scalar/bit-quantization branch because it generalizes 1-bit quantization to flexible bits per dimension with theoretical guarantees. |
| `raw/sources/papers/low-precision-quantization-knn-2021.pdf` | Anthony Ko et al., "Low-Precision Quantization for Efficient Nearest Neighbor Search" | 1 | Implementation-level integer quantization for KNN frameworks. Directly relevant to scalar quantization as a drop-in low-precision distance-computation layer. |
| `raw/sources/papers/norm-explicit-quantization-mips-2020.pdf` | Xinyan Dai et al., "Norm-Explicit Quantization: Improving Vector Quantization for Maximum Inner Product Search" | 1 | Relevant to inner-product search and TurboQuant-style concerns: it separates norm error from direction error and explicitly quantizes vector norms. |
| `raw/sources/papers/quantization-to-speedup-ann-2024.pdf` | Hao Peng, "Quantization to Speedup Approximate Nearest Neighbor Search" | 3 | Lower-priority supporting source on quantization-based ANN acceleration. Ingest mainly if it clarifies IVF-HNSW or adaptive termination around quantized candidates. |
| `raw/sources/papers/symphonyqg-2024.pdf` | Yutong Gou et al., "SymphonyQG: Towards Symphonious Integration of Quantization and Graph for Approximate Nearest Neighbor Search" | 2 | Modern quantization-plus-graph paper. Useful if the vault needs a bridge between RaBitQ-style quantization and graph ANN search quality. |
| `raw/sources/papers/aqr-hnsw-2026.pdf` | Ganap Ashit Tewary, Nrusinga Charan Gantayat, Jeff Zhang, "AQR-HNSW: Accelerating Approximate Nearest Neighbor Search via Density-aware Quantization and Multi-stage Re-ranking" | 3 | Recent preprint. Directly relevant to scalar/adaptive quantization and SIMD HNSW, but should be treated cautiously until claims are checked. |
| `raw/sources/papers/quantization-enhanced-hnsw-lavq-2025.pdf` | "Quantization-Enhanced HNSW for Scalable Approximate Vector Search" | 3 | Under-review/anonymous OpenReview paper. Relevant for percentile-clipped scalar quantization and AVX2 integer HNSW kernels; ingest only with low confidence. |
| `raw/sources/papers/information-theoretic-binarization-vector-search-2026.pdf` | Seyed Moein Abtahi et al., "From HNSW to Information-Theoretic Binarization: Rethinking the Architecture of Scalable Vector Search" | 3 | Recent binary-quantization/vector-search preprint. Include as a speculative direction for 1-bit search and HNSW alternatives, not as established evidence. |

## Ingest Result

The batch has been ingested into source notes, entity pages, [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md), [Scalar and Binary Quantization for ANN](../topics/scalar-and-binary-quantization-for-ann.md), [Vector Quantization](../topics/vector-quantization.md), and [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md).

The strongest immediate sources are PQ Fast Scan, Quicker ADC, the RaBitQ extension, Low-Precision Quantization for KNN, Norm-Explicit Quantization, and SymphonyQG. The 2025/2026 scalar/binary HNSW sources remain low-confidence exploratory inputs.

## Interpretation For The Vault

The current vault already has strong algorithmic coverage of [Product Quantization](../entities/product-quantization.md), [RaBitQ](../entities/rabitq.md), [TurboQuant](../entities/turboquant.md), and [FAISS](../entities/faiss.md). The missing layer is lower-level execution:

- PQ is not only a compression method; ADC performance depends on lookup-table placement, register layout, SIMD shuffle support, and cache traffic.
- Scalar/bit quantization is not only a memory-saving trick; it changes the distance kernel, the reranking strategy, the amount of residual/full-precision storage needed, and the recall-latency trade-off.
- Graph ANN systems have a tension between irregular traversal and vectorized distance kernels. SIMD helps most when candidate evaluation is batched or when quantized vectors make dense reranking cheap.
- Norm handling is especially important for inner-product search: papers such as norm-explicit quantization help explain why reconstruction MSE is not always the right objective.

## Acquisition Notes

- `pq-fast-scan-2015.pdf` was downloaded from the PVLDB-hosted copy because the ACM DOI PDF endpoint returned HTTP 403.
- `quantization-enhanced-hnsw-lavq-2025.pdf` is an anonymous OpenReview PDF and should remain low-confidence.
- `aqr-hnsw-2026.pdf` and `information-theoretic-binarization-vector-search-2026.pdf` are recent arXiv preprints and should be checked for implementation quality and benchmark comparability before being cited strongly.

## Open Questions

- Should the vault split scalar quantization into its own topic page, or keep it under [Vector Quantization](../topics/vector-quantization.md) until more sources are ingested?
- For the user's ANNS systems work, is the most important scalar-quantization target HNSW traversal vectors, IVF/PQ reranking vectors, or disk/second-tier raw-vector storage?
- Which benchmarks should be used to compare SIMD/PQ and scalar-quantized HNSW claims at matched recall: SIFT/Deep/GIST, BigANN, or modern embedding datasets?
