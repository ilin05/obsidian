---
id: gustann-2025
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
  - raw/sources/papers/gustann-2025.pdf
related:
  - gustann
  - fusionanns
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# High-Throughput, Cost-Effective Billion-Scale Vector Search with a Single GPU

## Bibliographic Note

Haodi Jiang, Hao Guo, Minhui Xie, Jiwu Shu, and Youyou Lu. 2025. *High-Throughput, Cost-Effective Billion-Scale Vector Search with a Single GPU*. Proc. ACM Manag. Data 3, 6 (SIGMOD), Article 334. DOI: `10.1145/3769799`.

The system name is [GustANN](../entities/gustann.md).

## Core Problem

GustANN targets high-throughput billion-scale ANNS when graph indexes live on SSDs. The paper's key diagnosis is that CPU-only on-SSD graph search does not saturate modern multi-SSD bandwidth, while naive GPU designs run into GPU-memory limits and PCIe inbound-bandwidth limits.

The paper positions the problem around cost/QPS rather than only latency: billion-scale vectors exceed practical DRAM capacity, SSDs are cheaper per GB, and graph indexes can be more throughput-efficient than cluster-based indexes if the system can keep SSDs and GPU busy.

## Design

GustANN uses a GPU-centric, CPU-assisted architecture.

- **Memory-efficient GPU graph kernels:** avoid large per-query visited tables by tolerating redundant work and then deduplicating in parallel. This lowers per-query GPU memory and allows much larger concurrent batches.
- **CPU-assisted selective transfer:** read SSD pages into CPU DRAM, then transfer only the needed graph node/record to GPU. This avoids GPU direct-access page granularity wasting scarce GPU inbound PCIe bandwidth.
- **Pivot search:** place a small pivot graph in GPU memory and use it to scatter queries to different graph regions, reducing inter-SSD load imbalance caused by single-entry graph traversal.

## Evaluation Facts

- Hardware: 2x Intel Xeon Gold 5420+, 256 GB DDR5, 6x Samsung PM1743 SSDs, NVIDIA A100-40GB, Ubuntu 22.04, CUDA 12.3.
- Datasets: SIFT1B, DEEP1B, SIFT100M, DEEP100M.
- Baselines: DiskANN, Starling, SPANN, BANG, RUMMY. Billion-scale comparison is mainly against DiskANN because several other systems exceed the setup memory budget.
- Headline: with one GPU and six SSDs, GustANN reports 247K billion-scale searches/s and up to 86.7% SSD utilization.
- Against DiskANN on billion-scale datasets at recall@10 = 0.9, GustANN reports 7.81x higher throughput on SIFT1B and 6.98x on DEEP1B.
- Against GPU systems, the paper reports at least 2.50x higher throughput and at least 2.62x better cost-effectiveness.
- Per-query GPU memory is reported as 40.22 KB for GustANN versus 480 KB for BANG; at 40K concurrent queries, GustANN uses 1.53 GB GPU memory versus 18.3 GB for BANG.

## Important Trade-off

GustANN trades latency for throughput. The paper reports about 140 ms latency at maximum throughput and says strict 10 ms-scale workloads may not reach maximum throughput. This makes it most relevant to high-throughput RAG/retrieval serving, not necessarily ultra-low-latency interactive ANN.

## Relevance To Memory-Tiered ANN

GustANN frames billion-scale ANN as a cross-tier scheduling problem, not just an index-design problem.

The strongest comparison point is conceptual:

- GustANN makes SSD bandwidth useful by moving graph traversal to GPU, shrinking per-query state, selectively transferring records, and load-balancing SSD access.

GustANN also creates a reviewer-facing baseline pressure for any future out-of-DRAM ANN paper: SSD+GPU systems are optimized for a different tier and hardware budget, but they share the same underlying concern that irregular vector search must be made bandwidth-efficient under concurrency.

## Follow-up Questions

- Can the pivot-search idea inspire CXL-side entry scattering or batch-level hot-region avoidance for HNSW?
- How should cost/QPS be compared across CXL memory, SSD+GPU, and DRAM+GPU systems when the storage tiers differ so much?
- Which workloads can tolerate GustANN's high-concurrency latency profile in exchange for higher throughput?
