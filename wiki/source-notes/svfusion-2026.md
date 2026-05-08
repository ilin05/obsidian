---
id: svfusion-2026
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - gpu
  - streaming
  - updates
  - memory-tier
source_count: 1
sources:
  - raw/sources/papers/svfusion-2026.pdf
related:
  - svfusion
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
  - bang
  - spfresh
  - fusionanns
  - gustann
confidence: medium
---

# SVFusion: A CPU-GPU Co-Processing Architecture for Large-Scale Real-Time Vector Search

## Bibliographic Note

Yuchen Peng, Dingyu Yang, Zhongle Xie, Ji Sun, Lidan Shou, Ke Chen, and Gang Chen. 2026. *SVFusion: A CPU-GPU Co-Processing Architecture for Large-Scale Real-Time Vector Search*. arXiv:2601.08528v1, dated 2026-01-13.

The system name is [SVFusion](../entities/svfusion.md).

## Problem

SVFusion targets streaming approximate nearest neighbor search (SANNS), where search, insertion, and deletion operations interleave on a continuously changing vector collection.

The paper frames a gap between:

- **CPU graph systems:** update-friendly but throughput-limited by CPU memory bandwidth and computation.
- **GPU graph systems:** high-throughput on static indexes, but constrained by GPU memory capacity and costly CPU-GPU movement under updates.
- **Hybrid compressed GPU methods:** reduce footprint but may lose accuracy and require codebook retraining under evolving data.

## Main Ideas

- **Hierarchical GPU-CPU-disk index:** GPU HBM caches hot full vectors and subgraphs; CPU memory maintains the complete graph and coordinates execution; disk stores cold vectors/graph state when CPU memory is exhausted.
- **CPU-GPU co-processing search:** graph traversal starts on GPU. Cache hits are processed on GPU; misses are either promoted to GPU or computed on CPU depending on transfer/computation trade-offs.
- **Workload-aware vector placement (WAVP):** placement decisions combine predicted future access, recent access, and graph-topology importance such as in-neighbor count.
- **GPU-parallel build and partition merge:** construction is inspired by CAGRA; large datasets are partitioned, built on GPU, and merged on CPU under bounded memory.
- **Streaming update protocol:** insertions use GPU search plus CPU neighbor refinement and reverse-edge insertion; deletions use lazy marking, affected-vertex repair, and periodic consolidation.
- **Concurrency control:** fine-grained locks, deletion bitsets, multi-version graph snapshots, and asynchronous CPU-to-GPU propagation keep searches consistent with interleaved updates.
- **Real-time coordination:** CUDA multi-stream execution, multi-CTA intra-query parallelism, and adaptive resource management target both high throughput and low tail latency.

## Evaluation Claims

The paper evaluates recall, search throughput, insert throughput, latency, and GPU cache miss rate under streaming workloads.

Reported comparison points include FreshDiskANN, HNSW, CAGRA, PilotANN, GGNN, and GPU-accelerated variants of CPU baselines. The paper reports up to 9.5x higher search throughput and 71.8x higher insertion throughput in its contribution summary, and also reports 1.3x to 50.7x lower latency across baseline comparisons.

The evaluation is especially useful because it treats update throughput, p99 latency, and cache miss rate as first-class measurements rather than only static recall-QPS.

## Relevance To This Vault

SVFusion fills a gap left explicit by [Graph-Based ANNS Survey 2021](graph-based-anns-survey-2021.md): the survey focuses on main-memory core graph algorithms and leaves GPU, SSD, distributed, structured, and dynamic-update settings as future directions.

It also extends the limitations of [ANN-Benchmarks](ann-benchmarks-2018.md). ANN-Benchmarks gives the in-memory speed-recall baseline protocol, but SVFusion shows that modern vector systems also need update freshness, heterogeneous memory residency, CPU-GPU transfer, and tail-latency metrics.

Positioning:

- Closest streaming/update references: [SPFresh](../entities/spfresh.md), FreshDiskANN, dynamic HNSW/NSG variants.
- Closest GPU/out-of-HBM references already in the vault: [BANG](../entities/bang.md), [RUMMY](../entities/rummy.md), [GustANN](../entities/gustann.md), and [FusionANNS](../entities/fusionanns.md).
- Closest graph-kernel reference to ingest next: CAGRA.

## Limits And Uncertainty

- This is an arXiv v1 paper; publication venue and independent reproduction were not checked here.
- The reported gains depend on A100-class GPU hardware, GPU memory size, workload skew, and streaming workload design.
- SVFusion extends CAGRA and compares against several GPU/dynamic systems that are not all fully ingested in this vault yet.
- The paper is a system design note as much as an ANN algorithm paper; claims should be compared under matched hardware, batch size, update ratio, and freshness guarantees.

## Follow-Up Candidates

- CAGRA: GPU graph construction/search primitive used as SVFusion's static GPU basis.
- FreshDiskANN: dynamic graph ANN baseline for streaming updates.
- SONG, GANNS, and GGNN: GPU graph-search lineage before CAGRA/SVFusion.
- PilotANN: memory-bounded GPU vector search, cited as a hybrid baseline.
- Big ANN / NeurIPS Streaming Challenge: benchmark protocol for streaming vector search.
