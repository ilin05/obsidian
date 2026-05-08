---
id: spann-2021
type: source-note
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - systems
  - inverted-index
  - memory-disk
  - billion-scale
source_count: 1
sources:
  - raw/sources/papers/spann-2021.pdf
related:
  - spann
  - diskann
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
  - disaggregated-memory-vector-search
confidence: high
---

# SPANN: Highly-Efficient Billion-scale Approximate Nearest Neighbor Search

**Authors:** Qi Chen, Bing Zhao, Haidong Wang, Mingqin Li, Chuanjie Liu, Zengzhong Li, Mao Yang, Jingdong Wang  
**Affiliations:** Microsoft, Peking University, Tencent, Baidu  
**Venue:** NeurIPS 2021 (arXiv 2111.08566v1)  
**Deployed:** Microsoft Bing (hundreds of billions of vectors)

## Core Contribution

SPANN is a memory-disk hybrid IVF (inverted index) system that achieves high recall at low latency and low memory cost for billion-scale ANN. It stores centroid navigating index and cluster centroids in DRAM; posting lists (vectors) on SSD. Three techniques address core challenges of inverted-index-on-SSD designs.

## System Design

**Index structure:**
- Data vectors X divided into N posting lists X₁,...,Xₙ.
- Centroids c₁,...,cₙ stored in memory (using SPTAG navigating index).
- Full posting lists stored on SSD.
- Search: find K nearest centroids → load corresponding posting lists into memory → fine-grained distance computation.

**Three key techniques:**

1. **Hierarchical balanced clustering (HBC):** standard KMeans produces highly unbalanced posting lists. HBC iteratively clusters into small groups until each posting list ≤ limit size, minimizing variance of list lengths (Equation 1 with balance penalty λ). Whole index buildable in ~2 minutes on one machine.

2. **Closure assignment:** boundary vectors that are close to multiple cluster boundaries are assigned to multiple nearest clusters (up to 8 replicas). Ensures boundary vectors are recalled even when only searching K posting lists. Controlled by ε₁ threshold. RNG rule further optimizes replica diversity.

3. **Query-aware dynamic pruning:** instead of always searching the same K posting lists, dynamically prune to those where Dist(q, cᵢⱼ) ≤ (1+ε₂)·Dist(q, c_nearest). Reduces disk I/O for easy queries while maintaining recall.

## Results

- 2× faster than DiskANN at equal recall@1 and recall@10 on SIFT1B, SPACEV1B, DEEP1B.
- 90% recall@1 in ~1ms; recall@10 in ~1ms (32GB memory budget).
- 10% original memory cost of DiskANN at same recall level.
- VQ capacity (latency×recall product) best among all tested systems.
- Distributed extension: partial search over posting lists scales naturally to cluster.

## DRAM+SSD vs DRAM+CXL Comparison

SPANN is a critical comparison point for CXL-aware ANN systems:

| Axis | SPANN | CXL-memory ANN design |
|---|---|---|
| Storage tier | SSD (high latency, sequential-friendly) | CXL (lower latency, random-access tolerant) |
| Index coarse stage | IVF centroids in DRAM | DRAM-local routing or cached routing state |
| Fine stage | full vectors from SSD posting lists | fine-grained CXL memory reads |
| Pruning | query-aware dynamic pruning | bound or schedule the expensive second-tier stage |
| Graph structure | inverted index (flat clustering) | graph or hybrid indexes may be practical |
| Update support | offline (no online updates described) | depends on target system |

## Relevance to Future CXL ANN Work

- SPANN is the dominant DRAM+SSD approach and a key baseline before CXL enters the picture.
- Any CXL ANN design must address "why CXL over SSD?": CXL offers lower random-access latency and finer granularity, while SSD favors sequential reads.
- SPANN's posting-list approach separates the "find candidates" stage from the "verify candidates" stage, a pattern shared by many coarse-to-exact systems.
- SPANN's posting-list length limitation is an example of capping the size of the expensive remote-access stage.

## Open Questions

- At what dataset scale and hardware cost does a CXL-memory approach outperform SPANN's IVF-on-SSD approach?
- Can SPANN-style dynamic pruning be ported to graph or hybrid indexes on newer memory tiers?
