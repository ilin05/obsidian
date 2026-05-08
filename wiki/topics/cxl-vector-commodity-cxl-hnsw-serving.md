---
id: cxl-vector-commodity-cxl-hnsw-serving
type: topic
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - cxl
  - hnsw
  - serving-runtime
source_count: 2
sources:
  - vldb-paper-outline.md
  - vldb-paper-intro-draft.md
related:
  - cxl-vector
  - cxl-vector-vldb-paper
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
  - disaggregated-memory-vector-search
confidence: high
---

# Commodity CXL HNSW Serving

## Summary

This page captures the refreshed CXL-Vector system design from the latest paper outline and introduction draft. The design is now explicitly about making HNSW serving work on commodity CXL memory without custom CXL endpoint compute.

## Failure Modes

The current paper motivates CXL-Vector through three compounding failures in naive CXL-HNSW:

- **Unconstrained remote raw-vector access:** node-centric HNSW layout drags raw vectors across PCIe during graph traversal.
- **Single-query blocking:** one worker drives one query through blocking remote loads, leaving no useful work to cover CXL stalls.
- **Unmanaged bandwidth pressure:** thread count becomes a bandwidth consumer, and adding threads worsens queuing once PCIe/CXL saturates.

## Design Components

**Layered placement:** build in a classical DRAM-friendly HNSW layout, then convert to a serving image where routing data stays in DRAM and raw vectors move to CXL.

**Coarse-to-exact search:** use SQ8 metadata/codes for upper routing and level-0 traversal, with AVX-512 VNNI for the hot level-0 consume loop.

**Bounded exact rerank:** make raw-vector CXL access a controlled final stage through `rerank_abs_limit`, `rerank_factor`, and `rerank_cap`.

**Morsel-driven execution:** execute batches through per-query state and issue/consume interleaving so a worker can make progress on one query while another waits for prefetched data.

## Evaluation Shape

The latest outline expects:

- Eight datasets from 1M to 500M vectors.
- HNSWLib, DiskANN-vamana, FAISS `IndexHNSWSQ`, and FAISS `IndexRefine` baselines.
- Thread-scaling and ablation evidence showing the combined effect of SQ8, scheduling, and bounded CXL access.
- Sensitivity sweeps for morsel size, rerank budget, `ef_search`, and patience.
- Kernel-level NUMA verification for the CXL-resident vector segment.

## Claim Discipline

The paper can strongly claim that CXL-Vector is a commodity-CXL software serving runtime. It should be careful when claiming uniqueness, because CXL-ANNS already explores CXL ANN with hardware offload. The distinction is commodity software versus custom endpoint acceleration.

## Related Pages

- [CXL-Vector](../entities/cxl-vector.md)
- [CXL-Vector VLDB Paper](../overview/cxl-vector-vldb-paper.md)
- [Second-tier Memory for Vector Search](second-tier-memory-for-vector-search.md)
- [Disaggregated Memory Vector Search](disaggregated-memory-vector-search.md)

