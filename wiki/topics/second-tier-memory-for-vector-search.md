---
id: second-tier-memory-for-vector-search
type: topic
status: active
created: 2026-04-08
updated: 2026-05-08
tags:
  - ann
  - systems
  - second-tier-memory
  - ssd
  - cxl
source_count: 5
sources:
  - raw/sources/papers/performance-index-size-dilemma-2024.pdf
  - raw/sources/papers/diskann-2019.pdf
  - raw/sources/papers/spann-2021.pdf
  - raw/sources/papers/cxl-memory-characterization-2023.pdf
  - vldb-paper-outline.md
related:
  - diskann
  - spann
  - cxl-vector
  - cxl-vector-commodity-cxl-hnsw-serving
  - disaggregated-memory-vector-search
  - approximate-nearest-neighbor-search
confidence: medium
---

# Second-tier Memory for Vector Search

## Summary

This topic tracks ANN designs that treat memory capacity as the bottleneck. SSD-centered indexes optimize for coarse-grained storage access, while CXL approaches can revisit finer-grained graph traversal because the memory tier is byte-addressable and far lower latency than SSD.

## Current View

- [DiskANN](../entities/diskann.md) is the canonical SSD-resident graph baseline using Vamana plus compression.
- [SPANN](../entities/spann.md) is the strongest DRAM+SSD IVF-style baseline and remains essential for positioning any CXL ANN paper.
- The performance/index-size dilemma paper argues that SSD access granularity drives throughput amplification in billion-scale search.
- CXL memory characterization provides the hardware basis for a different design point: random access is still costly compared with DRAM, but feasible enough that HNSW-style traversal can be redesigned rather than abandoned.
- The latest [CXL-Vector](../entities/cxl-vector.md) framing uses this gap directly: keep routing structures in DRAM, store raw vectors in CXL, and schedule the remaining exact-rerank access.

## Tier Comparison

| Tier | Latency | Access | ANN design pressure |
|---|---|---|---|
| Local DRAM | tens of ns | random-friendly | HNSW, ScaNN, FAISS in-memory |
| CXL memory | tens to low hundreds of ns depending on hardware | random-tolerant but bandwidth-sensitive | commodity-CXL HNSW serving with bounded remote stages |
| NVM/Optane | hundreds of ns | mixed | hybrid layouts |
| SSD/NVMe | 100-200 us | sequential-friendly | IVF posting lists, beam/search batching |

## Open Questions

- At what scale and hardware cost does DRAM+CXL outperform DRAM+SSD?
- Can SPANN-style pruning ideas transfer to CXL-Vector's rerank budget or query scheduling?
- How should memory footprint be compared when CXL reduces DRAM use but still consumes total attached memory?

## Related Pages

- [DiskANN](../entities/diskann.md) · [SPANN](../entities/spann.md) · [CXL-Vector](../entities/cxl-vector.md)
- [Commodity CXL HNSW Serving](cxl-vector-commodity-cxl-hnsw-serving.md)
- [Disaggregated Memory Vector Search](disaggregated-memory-vector-search.md)
- [Approximate Nearest Neighbor Search](approximate-nearest-neighbor-search.md)
- [Performance vs Index-size Dilemma Source Note](../source-notes/performance-index-size-dilemma-2024.md)
- [SPANN Source Note](../source-notes/spann-2021.md)
- [CXL Memory Characterization Source Note](../source-notes/cxl-memory-characterization-2023.md)
