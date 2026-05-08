---
id: faiss
type: entity
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - library
  - gpu
  - quantization
  - ivfpq
source_count: 2
sources:
  - raw/sources/papers/faiss-gpu-2017.pdf
  - raw/sources/papers/faiss-library-2025.pdf
related:
  - product-quantization
  - scann
  - spann
  - milvus
  - approximate-nearest-neighbor-search
  - vector-quantization
confidence: high
---

# FAISS

**Facebook AI Similarity Search**  
Meta FAIR (formerly Facebook AI Research)  
Open-source: https://github.com/facebookresearch/faiss

## What It Is

FAISS is the most widely deployed open-source library for vector similarity search. It is a C++ toolkit (with Python bindings) for building ANN indexes and searching them — not a database, not a service, not a distributed system. It is a building block.

## Core Design

Two-paper lineage:
- **Johnson et al. 2017** ("Billion-scale Similarity Search with GPUs"): GPU-optimized k-selection (WarpSelect) and IVFPQ on GPU. Established FAISS as capable of billion-scale search.
- **Douze et al. 2025** ("The Faiss Library"): comprehensive architecture description, index taxonomy, quantizer hierarchy, Pareto optimization framework.

## Index Taxonomy

| Index | Method | When to use |
|---|---|---|
| IndexFlat | Exact brute force | Ground truth, small dataset |
| IndexIVFFlat | Coarse k-means + flat | Moderate scale, high recall |
| IndexIVFPQ | Coarse k-means + PQ | Memory-constrained large scale |
| IndexIVFSQ8 | Coarse k-means + SQ8 | Fast approximate, low memory |
| IndexHNSW | Graph traversal | Low-latency single-query |
| IndexIVFRQ/AQ | Coarse + residual/additive Q | Best quality compressed |

## Key Algorithmic Contributions

- **WarpSelect:** GPU k-selection operating entirely in registers; fuseable with distance kernels; 1.6–2× faster than prior art.
- **IVFADC:** inverted file with asymmetric distance computation; coarse quantizer shortlists, PQ lookup tables score within lists.
- **IndexRefine:** fast approximate index to generate candidate shortlist → accurate reranker. This is a general coarse-to-exact pattern used across ANN systems.
- **Scalar quantizer (SQ8):** uniform per-dimension quantization from float32 to 1 byte. No codebook training needed. Used in IVF_SQ8.

## Performance

- 8.5× faster than prior GPU state of art (SIFT1M, k=100).
- k-NN graph on 95M vectors (YFCC100M) in 35 minutes on 4 GPUs.
- 5200+ citations; used as baseline in nearly every ANN systems paper.
- Foundation for Milvus, Weaviate, and most production vector databases.

## Scope and Limitations

- **Assumes data fits in RAM** (or GPU memory). No out-of-core support.
- **No concurrency:** not thread-safe for concurrent writes; no transaction management.
- Billion-scale on GPU requires 80+ GB GPU memory or multi-GPU sharding — expensive.
- Does not address CXL or heterogeneous memory tiers.

## Relation to ANN Systems

- FAISS HNSW is a canonical in-DRAM HNSW baseline.
- FAISS IVF_SQ8 provides an authoritative implementation reference for scalar-quantized ANN indexes.
- FAISS IndexRefine is a conceptual precursor to many coarse-to-exact pipelines.
