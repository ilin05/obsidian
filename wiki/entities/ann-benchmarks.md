---
id: ann-benchmarks
type: entity
status: active
created: 2026-04-10
updated: 2026-05-08
tags:
  - ann
  - benchmarking
  - evaluation
source_count: 1
sources:
  - raw/sources/papers/ann-benchmarks-2018.pdf
related:
  - approximate-nearest-neighbor-search
  - ann-benchmarking-methodology
  - faiss
  - scann
  - hnsw
  - flann
  - falconn
  - nn-descent
  - ngt-onng
confidence: high
---

# ANN-Benchmarks

**A Benchmarking Tool for Approximate Nearest Neighbor Algorithms**  
Aumüller, Bernhardsson, Faithfull — IT University of Copenhagen, 2018 (IS 2020)  
System: http://ann-benchmarks.com

## What It Is

ANN-Benchmarks is the standard benchmarking framework for in-memory approximate nearest neighbor algorithms. It provides Docker-based isolation, standardized metrics (recall, QPS, build time, memory), unified Python protocol, and automatic Pareto frontier visualization.

Its key methodological stance is that the benchmark measures concrete implementations, not algorithm names in the abstract.

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

## Benchmarking Lessons

- Use matched-recall Pareto frontiers rather than isolated QPS numbers.
- Report build time and index size because graph methods can buy high recall with expensive preprocessing.
- Separate batch throughput from online single-query latency.
- Treat dataset choice as a variable; rankings can change on synthetic or weak-global-structure datasets.
- Keep classical baselines visible: [FLANN](flann.md) represents tree/auto-tuning baselines, [FALCONN](falconn.md) represents angular LSH, and [NN-Descent](nn-descent.md)/KGraph-style methods represent KNN graph construction baselines.
- Use ANN-Benchmarks for DRAM-only implementation comparison, then add system-specific metrics for SSD, GPU, CXL, or streaming workloads.

## Scope

- **In-memory only.** All data must fit in RAM. Cannot benchmark SSD, CXL, or RDMA-resident systems directly.
- Benchmarks implementations, not algorithm ideas.
- ~50 algorithm implementations available.

## Relation to Out-of-DRAM ANN

- Must cite when reporting recall vs QPS on SIFT1M, GloVe, or other ANN-Benchmarks datasets.
- Batch mode is useful for throughput measurement, but ANN-Benchmarks assumes all data in DRAM.
- The Pareto frontier format is the expected visualization for recall-throughput results in the paper.
- Key limitation: ANN-Benchmarks cannot capture the cost of SSD/CXL/RDMA memory tiers, so its baselines represent an **optimistic upper bound** for in-DRAM systems.

## Related Pages

- [ANN-Benchmarks Source Note](../source-notes/ann-benchmarks-2018.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [FLANN](flann.md)
- [FALCONN](falconn.md)
- [NN-Descent](nn-descent.md)
