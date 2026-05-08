---
id: smartanns-2024
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - billion-scale
  - smartssd
  - near-data-processing
source_count: 1
sources:
  - raw/sources/papers/smartanns-2024.pdf
related:
  - smartanns
  - spann
  - diskann
  - ansmet
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# Scalable Billion-point Approximate Nearest Neighbor Search Using SmartSSDs

## Bibliographic Note

Bing Tian, Haikun Liu, Zhuohui Duan, Xiaofei Liao, Hai Jin, and Yu Zhang. 2024. *Scalable Billion-point Approximate Nearest Neighbor Search Using SmartSSDs*. USENIX ATC 2024.

Primary page: <https://www.usenix.org/conference/atc24/presentation/tian>.

The system name is [SmartANNS](../entities/smartanns.md).

## Core Problem

SmartANNS targets billion-point ANNS when in-memory indexes are too expensive and conventional SSD-based approaches spend too much time moving data between SSDs and host memory.

The paper's hardware premise is that SmartSSDs provide near-data processing: each device has on-board DRAM and FPGA compute. This can reduce host PCIe contention and host CPU/memory pressure, but naive multi-SmartSSD ANNS is inefficient because each device may scan too much and become load-imbalanced.

## Design

SmartANNS uses a host CPU plus SmartSSD cooperative hierarchy.

- **Hierarchical indexing:** keep shard centroids in host memory and store per-shard HNSW indexes on SmartSSDs.
- **Host coordination:** the CPU first selects likely shards through in-memory centroid traversal, then dispatches finer-grained search tasks to SmartSSDs.
- **Dynamic task scheduling:** place hot shards and schedule tasks to balance load across SmartSSDs while exploiting data reuse.
- **Learning-based shard pruning:** use offline training to estimate a query's required search scope, reducing unnecessary SmartSSD computations.
- **FPGA search kernels:** implement HNSW search on commercial Samsung SmartSSDs.

## Evaluation Facts

- Datasets include SIFT1B, SPACEV1B, DEEP1B, and Turing1B.
- At 90% Recall@10, SmartANNS reports 8.5x-10.7x QPS over CSDANNS.
- Across 88%-98% recall on SIFT1B, SmartANNS reports about 5.6x-9.8x QPS over CSDANNS.
- Energy efficiency improves by about 8.4x-9.4x over CSDANNS.
- Scaling from one to four SmartSSDs on SIFT1B increases QPS from 88 to 332, a 3.76x improvement.
- The paper also compares against SPANN and GPU baselines, arguing that SmartSSD-local internal PCIe avoids the host-PCIe bottleneck seen in traditional SSD-based systems.

## Relevance To Memory-Tiered ANN

SmartANNS is the main near-data-processing contrast point for SSD/CXL/GPU ANN systems. It shows a design that moves compute to the storage device rather than moving candidates up to CPU or GPU.

For future related work, SmartANNS should not be flattened into "SSD ANN." Its assumptions include SmartSSD FPGA compute and per-device internal bandwidth, which are different from commodity NVMe SSD systems such as [DiskANN](../entities/diskann.md), [SPANN](../entities/spann.md), [GustANN](../entities/gustann.md), or [FusionANNS](../entities/fusionanns.md).

## Follow-up Questions

- How portable are SmartANNS gains to newer commercial computational-storage devices?
- Does shard pruning remain robust under out-of-distribution queries?
- What is the fairest comparison axis: QPS per dollar, QPS per watt, or hardware availability?
