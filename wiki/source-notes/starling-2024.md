---
id: starling-2024
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - billion-scale
  - ssd
  - graph-index
source_count: 1
sources:
  - raw/sources/papers/starling-2024.pdf
related:
  - starling
  - diskann
  - spann
  - hnsw
  - nsg
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# Starling: An I/O-Efficient Disk-Resident Graph Index Framework for High-Dimensional Vector Similarity Search on Data Segment

## Bibliographic Note

Mengzhao Wang, Weizhi Xu, Xiaomeng Yi, Songlin Wu, Zhangyang Peng, Xiangyu Ke, Yunjun Gao, Xiaoliang Xu, Rentong Guo, Charles Xie, and collaborators. 2024. *Starling: An I/O-Efficient Disk-Resident Graph Index Framework for High-Dimensional Vector Similarity Search on Data Segment*. PACMMOD/SIGMOD 2024 family; arXiv:2401.02116.

Primary PDF: <https://arxiv.org/pdf/2401.02116>.

The system name is [Starling](../entities/starling.md).

## Core Problem

Starling targets high-dimensional vector similarity search inside a vector database data segment. A segment may hold tens of millions of vectors but has tight memory and disk budgets, such as 2 GB memory and 10 GB disk.

The paper argues that existing disk methods sit at two extremes:

- SPANN can be efficient but duplicates vectors, which can exceed segment capacity.
- DiskANN fits the segment budget better but suffers many random disk I/Os and poor block utilization.

## Design

Starling keeps the graph topology but reorganizes how disk and memory are used.

- **Reordered disk-based graph:** shuffle graph vertices so a vertex and its likely neighbors are stored in the same block, improving locality.
- **In-memory navigation graph:** sample a small fraction of vectors and build an in-memory graph to choose query-aware entry points before touching disk.
- **Block search strategy:** process loaded disk blocks as blocks rather than single vertices, using all relevant vertices in a loaded block.
- **Computation-specific optimizations:** pipeline disk reads and distance computation, use PQ short codes in memory, and prune loaded blocks.
- **Graph algorithm generality:** the framework can be applied to Vamana, NSG, and HNSW-style disk graphs.

## Evaluation Facts

- A segment with 2 GB memory and 10 GB disk can hold up to 33 million 128-dimensional vectors.
- In the segment setting, Starling reports latency below 1 ms with over 0.9 average precision and top-10 recall.
- The headline result reports 43.9x higher throughput and 98% lower query latency than state-of-the-art methods under the same accuracy.
- Against DiskANN for range search on BIGANN at AP = 0.9, Starling reports 43.9x higher throughput.
- On one billion BIGANN split into 31 segments, Starling reports more than 2x faster search than DiskANN in the high-recall regime above 0.96 recall.

## Relevance To Memory-Tiered ANN

Starling is important because it focuses on the physical data segment, not just a single global index. This matches how vector databases shard and segment data for scalability.

It also sharpens the distinction between "SSD graph fits" and "SSD graph is efficient." DiskANN makes graph search possible on SSD; Starling optimizes graph locality and block utilization within strict segment budgets.

## Follow-up Questions

- How does Starling's segment-level design interact with cluster-level query dispatch in vector databases?
- Can block-shuffling ideas improve SSD+GPU systems such as GustANN?
- Does the in-memory navigation graph duplicate or replace cache strategies used by DiskANN-like systems?
