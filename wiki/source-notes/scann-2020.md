---
id: scann-2020
type: source-note
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - quantization
  - mips
  - scann
source_count: 1
sources:
  - raw/sources/papers/scann-2020.pdf
related:
  - scann
  - product-quantization
  - vector-quantization
  - approximate-nearest-neighbor-search
confidence: high
---

# Accelerating Large-Scale Inference with Anisotropic Vector Quantization

**Authors:** Ruiqi Guo, Philip Sun, Erik Lindgren, Quan Geng, David Simcha, Felix Chern, Sanjiv Kumar  
**Affiliation:** Google Research  
**Venue:** ICML 2020 (arXiv 1908.10396v5)

## Core Contribution

ScaNN proposes a score-aware (anisotropic) quantization loss function for Maximum Inner Product Search (MIPS) that penalizes the component of quantization error parallel to the datapoint — the component that most distorts inner product rankings — more heavily than the orthogonal component.

## Key Insight

For MIPS, not all (query, datapoint) pairs are equally important: pairs where ⟨q, x⟩ is large are more likely to be top-ranked. Quantizing those high-inner-product datapoints accurately matters more. Standard reconstruction loss (MSE) treats all pairs equally, which is suboptimal for top-k retrieval.

**Score-aware quantization loss:**  
ℓ(xᵢ, x̃ᵢ, w) = h‖(w, ‖xᵢ‖) · ‖r‖(xᵢ, x̃ᵢ)‖² + h⊥(w, ‖xᵢ‖) · ‖r⊥(xᵢ, x̃ᵢ)‖²

where the weight function w is score-aware (monotonically non-decreasing), making h‖ > h⊥. This penalizes parallel residual (inner-product distortion) more than orthogonal residual.

## Algorithm

- Two-stage search: space partitioning (find relevant Voronoi partitions) + scoring (quantized distance computation within partitions).
- Score-aware loss applied to both PQ codebook learning and the partition assignment step.
- Iterative EM-style optimization: partition assignment → codebook update, with closed-form codeword update under anisotropic loss.
- Applied to product quantization directly; also applicable to other quantization methods.

## Results

- Fastest on speed-recall Pareto on Glove-1.2M at high recall (Recall@10 ≥ 0.9) against 11 baselines from ANN-Benchmarks.
- Better recall than QUIPS (variants) and LSQ at matched bitrate on Glove-1.2M.
- Significantly more accurate inner product value approximations vs reconstruction loss at all bitrates.

## Relevance to Out-of-DRAM ANN

- ScaNN is a strong in-DRAM quantized ANN baseline for high recall. It defines an in-DRAM performance ceiling for systems that move data to slower tiers.
- ScaNN's two-stage structure (partition → score) is conceptually relevant to coarse-to-exact search designs.
- Key contrast: ScaNN assumes all data fits in DRAM and optimizes for MIPS; it does not directly address SSD/CXL/RDMA memory tiers.
- Score-aware quantization may be relevant for coarse routing quality in future memory-tiered ANN systems.

## Open Questions

- Does score-aware loss generalize beyond MIPS to L2 distance search?
- At billion scale where data does not fit in DRAM, what is the right comparison between ScaNN-like in-DRAM search and SSD/CXL/RDMA-backed systems?
