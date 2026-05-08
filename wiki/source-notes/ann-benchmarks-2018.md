---
id: ann-benchmarks-2018
type: source-note
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
  - ann-benchmarks
  - approximate-nearest-neighbor-search
  - ann-benchmarking-methodology
  - faiss
  - scann
  - hnsw
  - nn-descent
  - flann
  - falconn
  - ngt-onng
confidence: high
---

# ANN-Benchmarks: A Benchmarking Tool for Approximate Nearest Neighbor Algorithms

**Authors:** Martin Aumüller, Erik Bernhardsson, Alexander Faithfull  
**Affiliations:** IT University of Copenhagen, Better Inc.  
**Venue:** SISAP 2017 (extended IS 2020 journal version, arXiv 1807.05614v2)  
**System:** http://ann-benchmarks.com

## Core Contribution

ANN-Benchmarks is a standardized benchmarking framework for in-memory approximate nearest neighbor algorithms. It provides Docker-based isolated evaluation, a unified Python protocol for algorithm wrappers, automatic dataset management, and speed-recall Pareto frontier visualization.

The framework intentionally benchmarks **implementations**, not abstract algorithm ideas. This matters because HNSW, FAISS, Annoy, or KGraph performance can change materially across implementation choices, build flags, and wrapper behavior.

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
- **HDF5 result storage:** records returned neighbors, query times, distances, build time, and metadata so new metrics can be computed without rerunning algorithms when possible.

## Datasets

Standard datasets used across ANN papers (fetched automatically):
- SIFT1M (128-dim float, L2)
- GIST1M (960-dim float, L2)
- GloVe-25, GloVe-100 (angular/cosine)
- NYTimes (256-dim angular)
- MNIST (784-dim euclidean)
- Fashion-MNIST

The paper's evaluation also includes Hamming-space variants such as SIFT-Hamming and Word2Bits, which are useful reminders that metric choice can change the ranking of implementations.

## Algorithm Coverage In The Paper

The evaluated families include:

- **Graph:** KGraph, SW-Graph, HNSW, PyNNDescent, PANNG/NGT.
- **Tree:** FLANN, BallTree, Annoy, RPForest, MRPT.
- **Hashing:** FALCONN and MPLSH.
- **Other:** Multi-Index Hashing for Hamming search and FAISS-IVF as an inverted-file baseline.

This source is therefore not only a benchmark paper; it is also a useful map of classical ANN families that should remain represented in the knowledge base.

## Important Findings

- Graph-based methods generally dominate high-recall in-memory recall-QPS on standard datasets.
- HNSW is often the fastest high-recall graph method in the paper's evaluation, while KGraph can closely match it at high recall on some datasets.
- FAISS-IVF is valuable when build time matters: it can build much faster than graph indexes and remains competitive below the highest-recall region.
- PANNG/NGT-style graph search is more robust on a synthetic dataset with weak global structure, showing that global graph navigability assumptions can fail.
- Batch mode changes the interpretation of throughput. FAISS-IVF on GPU and batched HNSW both improve substantially when query batches amortize overhead.
- Most strong implementations expose many low-level parameters instead of a direct target-recall interface; auto-tuning remains a durable evaluation problem.

## Scope and Limitations

- **In-memory only:** all datasets kept in RAM during benchmarking. Not designed for SSD-resident or CXL-resident indexes.
- Benchmarks implementations, not algorithm ideas (same algorithm, different implementation = different result).
- ~50 implementations across multiple algorithm families (LSH, tree, graph, quantization).
- Cannot directly benchmark billion-scale or out-of-core systems.

## Relevance to Out-of-DRAM ANN

- ANN-Benchmarks is the standard citation when reporting speed-recall curves on SIFT1M or GloVe. Must be cited when comparing against FAISS, ScaNN, HNSW baselines on these datasets.
- Batch mode evaluation is directly relevant to throughput-oriented systems: batch QPS is a different metric than single-query latency.
- Limitation: ANN-Benchmarks cannot evaluate CXL/SSD/RDMA-aware systems directly because it assumes in-memory storage.
- ANN-Benchmarks Pareto frontier framing is the expected presentation format for recall vs. throughput results.

## Connection To Newer Graph/System Papers

- [Graph-Based ANNS Survey 2021](graph-based-anns-survey-2021.md) complements ANN-Benchmarks by decomposing graph ANN methods into graph families and seven pipeline components.
- The vault now has primary notes for [NN-Descent](nn-descent-2011.md), [FLANN](flann-2014.md), [FALCONN](falconn-lsh-angular-2015.md), and [NGT/ONNG](ngt-onng-2018.md), covering several algorithm families named in ANN-Benchmarks.
- [SVFusion](svfusion-2026.md) shows the next evaluation layer for streaming GPU/CPU/disk systems: insertion throughput, deletion/repair behavior, p99 latency, and GPU cache miss rate must be reported alongside recall-QPS.
- For CXL/SSD/GPU ANN work, ANN-Benchmarks is best treated as an in-memory baseline protocol, not as a complete systems benchmark.

## Open Questions

- Should out-of-DRAM systems adopt billion-scale benchmarks such as BigANN instead of SIFT1M-scale ANN-Benchmarks datasets?
- How should single-node throughput be compared against in-memory HNSW baselines when memory tiers differ?
- Can ANN-Benchmarks-style Pareto visualization be extended to dynamic workloads without hiding recall drift or tail-latency spikes?
