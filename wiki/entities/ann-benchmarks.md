---
id: ann-benchmarks
type: entity
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - benchmarking
  - evaluation
source_count: 1
sources:
  - raw/sources/papers/ann-benchmarks-2018.pdf
related:
  - approximate-nearest-neighbor-search
  - faiss
  - scann
  - hnsw
confidence: high
---

# ANN-Benchmarks

**A Benchmarking Tool for Approximate Nearest Neighbor Algorithms**  
Aumüller, Bernhardsson, Faithfull — IT University of Copenhagen, 2018 (IS 2020)  
System: http://ann-benchmarks.com

## What It Is

ANN-Benchmarks is the standard benchmarking framework for in-memory approximate nearest neighbor algorithms. It provides Docker-based isolation, standardized metrics (recall, QPS, build time, memory), unified Python protocol, and automatic Pareto frontier visualization.

## Standard Datasets

- SIFT1M (128-dim float, L2): most common
- GIST1M (960-dim float, L2): higher dimensionality
- GloVe-25, GloVe-100 (cosine): word embeddings
- NYTimes-256 (cosine): document embeddings
- MNIST-784 (euclidean)
- Fashion-MNIST

## Key Metrics

- **Recall@k:** fraction of true k-NNs returned.
- **QPS:** queries per second (single-threaded by default).
- **Index build time:** offline preprocessing.
- **Index size:** memory footprint.
- **Pareto frontier:** speed-recall tradeoff curve. Upper-right dominates.

## Batch Mode

Entire query set given at once; useful for evaluating throughput-oriented systems (GPU, batched ANN). Reported separately from single-query latency.

## Scope

- **In-memory only.** All data must fit in RAM. Cannot benchmark SSD, CXL, or RDMA-resident systems directly.
- Benchmarks implementations, not algorithm ideas.
- ~50 algorithm implementations available.

## Relation to CXL-Vector

- Must cite when reporting recall vs QPS on SIFT1M, GloVe, or other ANN-Benchmarks datasets.
- Batch mode is analogous to CXL-Vector's throughput measurement, but ANN-Benchmarks assumes all data in DRAM — CXL-Vector evaluation requires separate protocol.
- The Pareto frontier format is the expected visualization for recall-throughput results in the paper.
- Key limitation to note in the paper: ANN-Benchmarks cannot capture the memory-tier cost of CXL-Vector experiments, so its baselines represent an **optimistic upper bound** for in-DRAM systems.
