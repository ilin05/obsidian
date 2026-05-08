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
  - cxl-vector
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

## Design Contrast with CXL-Vector

| Axis | SPANN | CXL-Vector |
|---|---|---|
| Second tier | SSD (100–200µs, sequential) | CXL (29–41ns, random-tolerant) |
| Index type | Inverted index (IVF) | Graph (HNSW) |
| Coarse stage | Centroid scan (SPTAG) | Graph traversal in DRAM |
| Fine stage | Sequential SSD read | Random CXL fetch (bounded) |
| Graph quality | Flat IVF clusters | HNSW hierarchical graph |
| Update model | Static / offline | Read-only serving runtime |

The core argument for CXL-Vector over SPANN: CXL's lower random-access latency (29ns vs 100+µs) enables HNSW-quality graph traversal that would be prohibitive on SSD. SPANN uses IVF because SSDs favor sequential reads; CXL-Vector can afford the irregular access pattern of HNSW.

## Relation to CXL-Vector

- SPANN is the **primary comparison baseline** for CXL-Vector. Any paper on CXL-aware ANN must show where CXL-Vector stands relative to SPANN.
- Both systems separate coarse candidate selection (DRAM-resident) from fine-grained verification (second-tier-resident).
- SPANN's query-aware dynamic pruning parallels CXL-Vector's bounded rerank + patience termination.
- The DRAM+SSD → DRAM+CXL transition is the central motivation for CXL-Vector's existence.
