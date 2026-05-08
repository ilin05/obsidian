---
id: fusionanns-2025
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - billion-scale
  - gpu
  - ssd
source_count: 1
sources:
  - raw/sources/papers/fusionanns-2025.pdf
related:
  - fusionanns
  - gustann
  - spann
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# Towards High-throughput and Low-latency Billion-scale Vector Search via CPU/GPU Collaborative Filtering and Re-ranking

## Bibliographic Note

Bing Tian, Haikun Liu, Yuhang Tang, Shihai Xiao, Zhuohui Duan, Xiaofei Liao, Hai Jin, Xuecang Zhang, Junhua Zhu, and Yu Zhang. 2025. *Towards High-throughput and Low-latency Billion-scale Vector Search via CPU/GPU Collaborative Filtering and Re-ranking*. FAST 2025, pp. 171-185.

The system name is [FusionANNS](../entities/fusionanns.md).

## Core Problem

FusionANNS targets the triangle of throughput, latency, cost, and accuracy for billion-scale ANNS. The paper argues that existing approaches tend to solve only part of the problem:

- SSD hierarchical indexing reduces memory cost but can bottleneck on concurrent I/O.
- Product quantization reduces footprint but may lose accuracy and require reranking.
- GPU acceleration helps distance computation but can make data movement across SSD, CPU memory, and GPU HBM the dominant bottleneck.

The paper's central claim is that these techniques need to be co-designed rather than simply stacked.

## Design

FusionANNS uses CPU/GPU collaborative filtering and reranking.

- **Multi-tiered index:** raw vectors are on SSDs, compressed PQ vectors are in GPU HBM, and vector IDs plus the navigation graph are in host memory. The CPU sends vector IDs, not vector contents, to the GPU.
- **Heuristic reranking:** split reranking into mini-batches and stop later mini-batches when feedback suggests limited accuracy gain. This aims to avoid unnecessary raw-vector I/O and CPU work.
- **Redundancy-aware I/O deduplication:** store similar raw vectors compactly and merge same-page raw-vector requests within and across mini-batches.

## Evaluation Facts

- Hardware: two Intel Xeon CPUs, 64 cores, 1 TB main memory, NVIDIA V100 32 GB, Samsung 990Pro 2 TB SSD.
- For FusionANNS and SSD baselines, the experiments use only 64 GB main memory for in-memory index and intermediate results.
- Datasets: SIFT1B, SPACEV1B, DEEP1B.
- Baselines: SPANN, DiskANN, RUMMY, plus GPU-accelerated SPANN/DiskANN variants.
- At recall@10 = 90%, FusionANNS reports 9.4x-13.1x QPS over SPANN and 3.2x-4.3x over DiskANN.
- Compared with RUMMY, FusionANNS reports 2x-4.9x higher QPS while maintaining low latency.
- Multi-tiered indexing with GPU reports 5.9x-6.8x QPS over SPANN; heuristic reranking and I/O deduplication further improve latency/QPS by up to 39% and 17%.
- I/O numbers are reduced by up to 30% with heuristic reranking and up to 23% with redundancy-aware I/O deduplication.
- Reported cost efficiency improves by 5.67x-8.78x over SPANN, 2x-2.5x over DiskANN, and 2.25x-6.82x over RUMMY.

## Relevance To Memory-Tiered ANN

FusionANNS isolates the expensive accurate stage and then reduces its I/O cost.

The closest conceptual link is reranking:

- FusionANNS keeps compressed vectors in HBM, then performs selective raw-vector reranking from SSD.

FusionANNS's I/O deduplication is also a useful pattern for future memory-tiered systems: it deduplicates SSD page reads for compactly stored similar vectors, exploiting repeated candidates in the accurate stage.

## Follow-up Questions

- Does FusionANNS's heuristic reranking imply a useful stopping policy for exact-rerank budgets in other systems?
- Which SSD+GPU collaborative systems should be direct baselines versus adjacent related work?
- Can FusionANNS's mini-batch reranking control model inform batching or coalescing decisions in other memory tiers?
