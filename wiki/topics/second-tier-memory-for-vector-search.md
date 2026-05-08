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
source_count: 11
sources:
  - raw/sources/papers/performance-index-size-dilemma-2024.pdf
  - raw/sources/papers/diskann-2019.pdf
  - raw/sources/papers/spann-2021.pdf
  - raw/sources/papers/cxl-memory-characterization-2023.pdf
  - raw/sources/papers/gustann-2025.pdf
  - raw/sources/papers/fusionanns-2025.pdf
  - raw/sources/papers/rummy-2024.pdf
  - raw/sources/papers/smartanns-2024.pdf
  - raw/sources/papers/starling-2024.pdf
  - raw/sources/papers/bang-2024.pdf
  - raw/sources/papers/spfresh-2023.pdf
related:
  - diskann
  - spann
  - gustann
  - fusionanns
  - rummy
  - smartanns
  - starling
  - bang
  - spfresh
  - disaggregated-memory-vector-search
  - approximate-nearest-neighbor-search
confidence: medium
---

# Second-tier Memory for Vector Search

## Summary

This topic tracks ANN designs that treat memory capacity as the bottleneck. SSD-centered indexes optimize for coarse-grained storage access, GPU/host-memory systems optimize PCIe scheduling, SmartSSD systems move compute into storage, and CXL approaches can revisit finer-grained graph traversal because the memory tier is byte-addressable and far lower latency than SSD.

## Current View

- [DiskANN](../entities/diskann.md) is the canonical SSD-resident graph baseline using Vamana plus compression.
- [SPANN](../entities/spann.md) is the strongest DRAM+SSD IVF-style baseline and remains essential for positioning any CXL ANN paper.
- [Starling](../entities/starling.md) refines disk-resident graph search for vector database segments by combining in-memory navigation graphs, reordered disk graph layout, and block search.
- [SPFresh](../entities/spfresh.md) adds the update-freshness dimension for disk-based vector search through in-place incremental rebalancing.
- [RUMMY](../entities/rummy.md) and [BANG](../entities/bang.md) cover host-memory plus GPU execution when the full dataset or graph cannot fit in GPU memory.
- [GustANN](../entities/gustann.md) and [FusionANNS](../entities/fusionanns.md) add the SSD+GPU branch: both reduce the cost of billion-scale search by keeping large data on SSD while using GPU selectively.
- [SmartANNS](../entities/smartanns.md) adds the SmartSSD/NDP branch, where computation is moved into commercial computational-storage devices.
- The performance/index-size dilemma paper argues that SSD access granularity drives throughput amplification in billion-scale search.
- CXL memory characterization provides the hardware basis for a different design point: random access is still costly compared with DRAM, but feasible enough that HNSW-style traversal can be redesigned rather than abandoned.

## Tier Comparison

| Tier | Latency | Access | ANN design pressure |
|---|---|---|---|
| Local DRAM | tens of ns | random-friendly | HNSW, ScaNN, FAISS in-memory |
| CXL memory | tens to low hundreds of ns depending on hardware | random-tolerant but bandwidth-sensitive | CXL-aware ANN placement, caching, scheduling, or device-side co-design |
| NVM/Optane | hundreds of ns | mixed | hybrid layouts |
| SSD/NVMe | 100-200 us | sequential-friendly | IVF posting lists, beam/search batching |
| SSD + GPU | SSD latency plus PCIe transfer | high throughput if batched and transfer-efficient | GustANN/FusionANNS-style collaborative filtering, selective transfer, and rerank deduplication |
| Host memory + GPU | DRAM latency plus PCIe transfer | high throughput if transfer/computation overlap | RUMMY/BANG-style batching, compressed GPU state, and CPU-GPU pipelining |
| SmartSSD/NDP | local SSD/internal-device bandwidth plus FPGA compute | device-local parallelism | SmartANNS-style host coordination, shard pruning, and load-balanced near-data search |

## Open Questions

- At what scale and hardware cost does DRAM+CXL outperform DRAM+SSD?
- Can SPANN-style pruning ideas transfer to CXL-memory ANN systems?
- Can FusionANNS/GustANN-style batching and deduplication ideas transfer across storage and memory tiers without violating latency goals?
- How should memory footprint be compared when CXL reduces DRAM use but still consumes total attached memory?
- How should fresh updates be compared against static-index systems in the same memory-tier taxonomy?

## Related Pages

- [DiskANN](../entities/diskann.md) · [SPANN](../entities/spann.md) · [Starling](../entities/starling.md) · [SPFresh](../entities/spfresh.md)
- [RUMMY](../entities/rummy.md) · [BANG](../entities/bang.md) · [GustANN](../entities/gustann.md) · [FusionANNS](../entities/fusionanns.md) · [SmartANNS](../entities/smartanns.md)
- [Disaggregated Memory Vector Search](disaggregated-memory-vector-search.md)
- [Approximate Nearest Neighbor Search](approximate-nearest-neighbor-search.md)
- [Performance vs Index-size Dilemma Source Note](../source-notes/performance-index-size-dilemma-2024.md)
- [SPANN Source Note](../source-notes/spann-2021.md)
- [GustANN Source Note](../source-notes/gustann-2025.md)
- [FusionANNS Source Note](../source-notes/fusionanns-2025.md)
- [CXL Memory Characterization Source Note](../source-notes/cxl-memory-characterization-2023.md)
