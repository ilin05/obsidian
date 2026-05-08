---
id: scann
type: entity
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - quantization
  - mips
  - google
source_count: 1
sources:
  - raw/sources/papers/scann-2020.pdf
related:
  - faiss
  - product-quantization
  - vector-quantization
  - approximate-nearest-neighbor-search
confidence: high
---

# ScaNN

**Scalable Nearest Neighbors**  
Google Research, 2020  
Open-source: https://github.com/google-research/google-research/tree/master/scann

## What It Is

ScaNN is a high-performance in-memory ANN library from Google Research. Its key innovation is **anisotropic (score-aware) quantization** — a loss function for codebook learning that is optimized for Maximum Inner Product Search (MIPS) rather than generic reconstruction error.

## Core Algorithm

**Score-aware quantization loss:**
- Observation: for MIPS, pairs with high inner product ⟨q, x⟩ matter more. Quantization error that distorts those pairs' ranking is more costly.
- **Parallel residual error** (r‖): component of quantization error along the datapoint — distorts inner product directly.
- **Orthogonal residual error** (r⊥): component orthogonal to datapoint — less harmful to ranking.
- Score-aware loss weights r‖ more heavily than r⊥, leading to better MIPS recall at equal bitrate.

**Two-stage search:**
1. Space partitioning: find relevant Voronoi partitions (Voronoi cells around centroids).
2. Scoring: quantized inner-product computation within partitions.

Score-aware loss applied to both PQ codebook learning and partition assignment.

## Performance

- **Fastest** on ANN-Benchmarks Glove-1.2M speed-recall Pareto at high recall (Recall@10 ≥ 0.9).
- Outperforms QUIPS, LSQ at matched bitrate.
- State-of-the-art MIPS retrieval at both 100-bit and 200-bit compression on Glove-1.2M.

## Scope

- In-memory only: assumes all data fits in DRAM.
- MIPS (cosine/inner product) primary focus; L2 via reduction.
- Does not address billion-scale out-of-memory scenarios or heterogeneous memory tiers.

## Relation to CXL-Vector

- ScaNN is the strongest **in-DRAM** quantized ANN baseline. It defines the DRAM performance ceiling.
- ScaNN's two-stage pipeline (partition → score) is structurally analogous to CXL-Vector's coarse-SQ8-routing → exact-remote-rerank.
- Key contrast: ScaNN assumes uniform flat DRAM; CXL-Vector addresses the case where data exceeds DRAM and resides in CXL.
- Score-aware loss is potentially applicable to improving CXL-Vector's SQ8 coarse routing quality (open research direction).
