---
id: faiss-library-2025
type: source-note
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - faiss
  - quantization
  - library
source_count: 1
sources:
  - raw/sources/papers/faiss-library-2025.pdf
related:
  - faiss
  - product-quantization
  - approximate-nearest-neighbor-search
  - vector-quantization
confidence: high
---

# The Faiss Library

**Authors:** Matthijs Douze, Alexandr Guzhva, Chengqi Deng, Jeff Johnson, Gergely Szilvasy, Pierre-Emmanuel Mazaré, Maria Lomeli, Lucas Hosseini, Hervé Jégou  
**Affiliation:** Meta FAIR, Zilliz, DeepSeek, Skip Labs  
**Venue:** arXiv 2401.08281v4, Oct 2025

## Core Contribution

Comprehensive description of the Faiss library architecture, design philosophy, index taxonomy, quantizer hierarchy, and optimization strategy. Faiss is an open-source C++ toolkit for vector similarity search without concurrency, data-extraction, or network serving — it is a building block for full systems.

## Design Philosophy

- **Not a database:** no concurrent writes, no transactions, no sharding, no query optimization.
- **Embedding contract:** distance metric is agreed between extractor and index; index is pure geometry.
- **Tradeoff axes:** accuracy, speed, memory usage — navigated via Pareto frontier.
- **IndexRefine pattern:** fast approximate index shortlists candidates; accurate index reranks. This is a general coarse-to-exact pipeline pattern.

## Index Taxonomy

| Index | Coarse | Fine | Notes |
|---|---|---|---|
| IndexFlat | — | exact | brute force baseline |
| IndexIVFFlat | k-means | flat | inverted lists, exact within list |
| IndexIVFPQ | k-means | PQ | compressed domain search |
| IndexIVFSQ | k-means | scalar | 8/16/32-bit uniform |
| IndexHNSW | — | exact | graph-based, in-RAM |
| IndexIVFRQ | k-means | residual | residual quantizer |
| IndexIVFAQ | k-means | additive | additive quantizer |

## Quantizer Hierarchy

- **Scalar quantizer (SQ):** SQ4, SQ6, SQ8, SQ16, SQfp16. SQ8 maps 4-byte float to 1-byte integer via uniform quantization per dimension. Fast to encode and decode; no pre-trained codebook needed.
- **Product quantizer (PQ):** splits vector into M subspaces, each independently quantized to k codewords. M×log₂(k) bits per vector.
- **Residual quantizer (RQ):** sequential PQ stages applied to residuals.
- **Additive quantizer (AQ):** extends RQ with beam-search encoding for better quality.

## SQ8 and Compressed-Domain Search

- IVF_SQ8 uses scalar quantizer as the fine quantizer inside inverted lists.
- Compressed-domain distance: precomputed lookup tables allow O(dk+mn) distance computation vs O(nd) for exact.
- SQ8 offers 4× compression vs float32 with low encoding/decoding cost.

## Pareto Frontier Navigation

- Faiss defines "search-time settings" as discrete hyperparameter tuples (nprobe, etc.).
- Pareto-optimal set: settings where no other point dominates in both speed and accuracy.
- Pareto pruning: monotone property exploited to prune large hyperparameter grid efficiently.
- IndexRefineFlatL2: fast approximate shortlist + flat exact rerank; useful when accuracy matters more than memory.

## Software Architecture

- C++ core with no external dependencies; Python wrapper via SWIG.
- Supports CPU (SSE/AVX/AVX2/AVX512) and GPU (CUDA).
- IndexFactory string spec for easy construction (e.g., "IVF1024,PQ16").
- 5200+ citation count as of paper writing time.

## Relevance to ANN Systems

- Definitive reference for FAISS as a baseline system.
- SQ8 quantizer description provides a canonical citation for scalar-quantized ANN metadata.
- IndexRefine is a general coarse-to-exact execution split.
- HNSW support in Faiss defines a graph-index baseline for in-memory systems.

## Open Questions

- How does Faiss HNSW compare at billion scale vs DiskANN/SPANN where the index doesn't fit in DRAM?
- What is the overhead of SQ8 re-scoring vs full-precision rerank, and how does this scale when data moves to a slower memory or storage tier?
