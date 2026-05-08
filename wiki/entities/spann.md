---
id: spann
type: entity
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - systems
  - billion-scale
  - memory-disk
  - inverted-index
  - microsoft
source_count: 1
sources:
  - raw/sources/papers/spann-2021.pdf
related:
  - diskann
  - faiss
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
  - disaggregated-memory-vector-search
confidence: high
---

# SPANN

**Highly-Efficient Billion-scale Approximate Nearest Neighbor Search**  
Microsoft Research, NeurIPS 2021  
Deployed: Microsoft Bing (hundreds of billions of vectors)  
Open-source: https://github.com/microsoft/SPTAG

## What It Is

SPANN is a memory-disk hybrid ANN system built on an inverted index (IVF) structure. It stores cluster centroids and a navigating index (SPTAG) in DRAM, and posting lists (full vectors grouped by nearest centroid) on SSD. It achieves high recall with low memory and competitive latency compared to DiskANN.

## Key Techniques

**1. Hierarchical balanced clustering (HBC):**
- Standard KMeans produces unbalanced posting lists → variable SSD read sizes.
- HBC iteratively clusters until each posting list ≤ limit size, minimizing length variance.
- ~16% of vectors as centroids; whole index builds in ~2 minutes.

**2. Closure assignment:**
- Boundary vectors assigned to multiple nearby clusters (up to 8 replicas via ε₁ threshold).
- RNG rule ensures replicas cover different directions to maximize recall coverage.
- Trades slightly more SSD storage for significantly better recall at boundary regions.

**3. Query-aware dynamic pruning:**
- Instead of always reading K posting lists, only read lists where centroid distance ≤ (1+ε₂)×nearest.
- Easy queries access fewer lists → lower latency; hard queries access more → higher recall.

## Performance

- 2× faster than DiskANN at equal recall@1 on SIFT1B, SPACEV1B, DEEP1B.
- 90% recall@1 in ~1ms with 32GB memory.
- 10% of DiskANN's memory consumption at same recall level.
- VQ capacity (vectors·queries/second) best among tested systems.

## Design Contrast With CXL Memory

| Axis | SPANN | CXL-memory ANN design |
|---|---|---|
| Second tier | SSD (100-200us, sequential) | CXL (tens to low hundreds of ns depending on hardware) |
| Index type | Inverted index (IVF) | Often graph or hybrid indexes |
| Coarse stage | Centroid scan (SPTAG) | DRAM-local routing or cached routing state |
| Fine stage | Sequential SSD read | Fine-grained memory reads |
| Access pressure | page/list-oriented | byte-addressable but bandwidth-sensitive |
| Design pressure | sequentialize and prune SSD I/O | control random access and bandwidth pressure |

The core design contrast: CXL's lower random-access latency can make graph-style traversal more plausible than on SSD, while SPANN uses IVF because SSDs favor coarse sequential reads.

## Relation to Future CXL ANN Work

- SPANN is a primary comparison point for any CXL-aware ANN system.
- Both systems separate coarse candidate selection (DRAM-resident) from fine-grained verification (second-tier-resident).
- SPANN's query-aware dynamic pruning is relevant to any system with an expensive second-tier verification stage.
