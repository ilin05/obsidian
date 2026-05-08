---
id: faiss-gpu-2017
type: source-note
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - gpu
  - faiss
  - ivfpq
  - quantization
source_count: 1
sources:
  - raw/sources/papers/faiss-gpu-2017.pdf
related:
  - faiss
  - product-quantization
  - approximate-nearest-neighbor-search
confidence: high
---

# Billion-scale Similarity Search with GPUs

**Authors:** Jeff Johnson, Matthijs Douze, Hervé Jégou  
**Affiliation:** Facebook AI Research  
**Venue:** arXiv 2017 (published IEEE TPAMI)

## Core Contribution

A GPU-optimized similarity search system built around two novel components: a near-optimal GPU k-selection algorithm (WarpSelect) and a GPU-resident IVFPQ indexing pipeline. Together they achieve 8.5× speedup over prior GPU state of the art, enabling billion-scale exact and approximate kNN on a multi-GPU server.

## WarpSelect: GPU k-Selection

- Standard heap-based k-selection is serial and cache-inefficient on GPUs.
- WarpSelect operates entirely in registers and warp-level shared memory — no global memory synchronization during selection.
- Uses odd-size merge networks (bitonic merge variants) for arbitrary k, not just power-of-2.
- Can be fused with distance computation kernels, eliminating intermediate write-backs.
- Achieves 55% of peak GPU memory bandwidth; 1.62× faster than faiss-select at k=100, 2.01× at k=1000.

## IVFPQ on GPU

- Two-stage: coarse quantizer (k-means centroid scan) narrows search to τ inverted lists; PQ lookup tables compute distances within each list.
- Distance computation decomposes into three terms; terms 1 and 3 are query-independent and precomputed.
- Batch processing: n_q queries batched together, tiled so that each tile fits in L3/shared memory.
- PQ codes are contiguous per inverted list; scanning is SIMD-friendly.
- Exact search uses matrix multiplication (cuBLAS GEMM) and k-selection fusion.

## Multi-GPU

- **Replication:** index fits on a single GPU, replicated across R GPUs. Each GPU handles n_q/R queries; results merged. Near-linear speedup.
- **Sharding:** index too large for one GPU; partitioned across S GPUs. Each shard handles full query set; results merged. Also near-linear speedup for large S.

## Key Results

- k-NN graph on 95M YFCC100M images built in 35 minutes on 4 Titan X GPUs (12 hours on 1 GPU × CPU state of art).
- SIFT1M, k=100: 8.5× faster than prior GPU implementation.
- Billion-scale 128-dim float vectors: processed on 4 GPU server in under 12 hours.

## Relevance to CXL-Vector

- Establishes FAISS/IVFPQ as the canonical ANN baseline; must be cited in any paper on ANN serving.
- WarpSelect pattern (register-resident selection, kernel fusion) is conceptually analogous to CXL-Vector's design goal of keeping hot state local and avoiding remote traffic per-query.
- IVFPQ coarse-to-exact structure is the in-DRAM precursor to SPANN (DRAM+SSD) and CXL-Vector (DRAM+CXL) designs.
- GPU-centric design assumes all data fits in fast GPU memory; CXL-Vector addresses the regime where data exceeds local DRAM.

## Open Questions

- How far does GPU IVFPQ scale before the coarse quantizer centroid scan becomes the bottleneck?
- What is the right comparison point for a CPU+CXL system vs a multi-GPU cluster?
