---
id: ann-benchmarks-2018
type: source-note
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
  - ann-benchmarks
  - approximate-nearest-neighbor-search
  - faiss
  - scann
confidence: high
---

# ANN-Benchmarks: A Benchmarking Tool for Approximate Nearest Neighbor Algorithms

**Authors:** Martin Aumüller, Erik Bernhardsson, Alexander Faithfull  
**Affiliations:** IT University of Copenhagen, Better Inc.  
**Venue:** SISAP 2017 (extended IS 2020 journal version, arXiv 1807.05614v2)  
**System:** http://ann-benchmarks.com

## Core Contribution

ANN-Benchmarks is a standardized benchmarking framework for in-memory approximate nearest neighbor algorithms. It provides Docker-based isolated evaluation, a unified Python protocol for algorithm wrappers, automatic dataset management, and speed-recall Pareto frontier visualization.

## Quality Metrics

- **Recall:** fraction of true k-NNs in the returned result set. Distance-based definition: p is included if dist(p, q) ≤ dist(p_k*, q) (k-th true NN distance threshold).
- **ε-approximate recall:** p included if dist(p, q) ≤ (1+ε)·dist(p_k*, q).
- **QPS:** queries per second.
- **Index build time:** preprocessing time (independent of query count).
- **Memory usage:** index size after preprocessing.

## Performance Measure Framework

Two axes: preprocessing (build time, index size) and query-time (QPS, recall). The Pareto frontier over (QPS, recall) is the primary visualization — algorithms closer to the upper-right are better.

## System Design

- **Docker isolation:** each algorithm runs in a container, preventing interference.
- **YAML configuration:** algorithm instances defined via constructor arguments and query-args separately (build once, query with multiple parameter settings).
- **Batch mode:** full query set given at once; useful for GPU algorithms and throughput-oriented systems.
- **Local mode:** for development, runs outside Docker for faster iteration.

## Datasets

Standard datasets used across ANN papers (fetched automatically):
- SIFT1M (128-dim float, L2)
- GIST1M (960-dim float, L2)
- GloVe-25, GloVe-100 (angular/cosine)
- NYTimes (256-dim angular)
- MNIST (784-dim euclidean)
- Fashion-MNIST

## Scope and Limitations

- **In-memory only:** all datasets kept in RAM during benchmarking. Not designed for SSD-resident or CXL-resident indexes.
- Benchmarks implementations, not algorithm ideas (same algorithm, different implementation = different result).
- ~50 implementations across multiple algorithm families (LSH, tree, graph, quantization).
- Cannot directly benchmark billion-scale or out-of-core systems.

## Relevance to CXL-Vector

- ANN-Benchmarks is the standard citation when reporting speed-recall curves on SIFT1M or GloVe. Must be cited when comparing against FAISS, ScaNN, HNSW baselines on these datasets.
- The batch mode evaluation is directly relevant to CXL-Vector's morsel-driven throughput evaluation: batch QPS is a different metric than single-query latency.
- Limitation: ANN-Benchmarks cannot evaluate CXL-aware systems because it assumes in-memory storage. CXL-Vector evaluation requires a separate protocol, which should be noted in the paper.
- ANN-Benchmarks Pareto frontier framing is the expected presentation format for recall vs. throughput results.

## Open Questions

- Should CXL-Vector evaluation adopt a billion-scale benchmark (BigANN) instead of SIFT1M-scale ANN-Benchmarks datasets?
- How to fairly compare single-node CXL-Vector throughput vs in-memory HNSW baselines when memory tiers differ?
