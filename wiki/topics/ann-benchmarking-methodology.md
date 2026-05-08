---
id: ann-benchmarking-methodology
type: topic
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - benchmarking
  - evaluation
  - methodology
source_count: 11
sources:
  - raw/sources/papers/ann-benchmarks-2018.pdf
  - raw/sources/papers/graph-based-anns-survey-2021.pdf
  - raw/sources/papers/svfusion-2026.pdf
  - raw/sources/papers/nn-descent-2011.pdf
  - raw/sources/papers/gnns-knn-graph-2011.pdf
  - raw/sources/papers/efanna-2016.pdf
  - raw/sources/papers/fanng-2016.pdf
  - raw/sources/papers/dpg-ann-experiments-2016.pdf
  - raw/sources/papers/ngt-onng-2018.pdf
  - raw/sources/papers/flann-2014.pdf
  - raw/sources/papers/falconn-lsh-angular-2015.pdf
related:
  - ann-benchmarks
  - approximate-nearest-neighbor-search
  - proximity-graph-theory-for-ann
  - svfusion
  - second-tier-memory-for-vector-search
  - flann
  - falconn
  - nn-descent
  - diversified-proximity-graph
  - ngt-onng
confidence: high
---

# ANN Benchmarking Methodology

## Summary

ANN evaluation should separate three questions that are often mixed together:

- **Implementation benchmarking:** how a concrete implementation performs under a standardized harness.
- **Algorithm/component analysis:** which graph, routing, pruning, seeding, or construction component causes the performance.
- **System benchmarking:** how performance changes when vectors move across DRAM, GPU HBM, SSD, CXL, or other tiers under static or streaming workloads.

[ANN-Benchmarks](../entities/ann-benchmarks.md) anchors the first question. [Graph-Based ANNS Survey 2021](../source-notes/graph-based-anns-survey-2021.md) anchors the second. [FLANN](../entities/flann.md) and [FALCONN](../entities/falconn.md) anchor important non-graph baseline families, while [NN-Descent](../entities/nn-descent.md), [EFANNA](../entities/efanna.md), [FANNG](../entities/fanng.md), [DPG](../entities/diversified-proximity-graph.md), and [NGT/ONNG](../entities/ngt-onng.md) provide primary-source coverage for graph components. [SVFusion](../entities/svfusion.md), [SPFresh](../entities/spfresh.md), [BANG](../entities/bang.md), [GustANN](../entities/gustann.md), and [FusionANNS](../entities/fusionanns.md) show why the third needs additional metrics beyond in-memory recall-QPS.

## Metric Layers

| Layer | Metrics | Notes |
|---|---|---|
| Query quality | Recall@k, epsilon recall, distance ratio, position-aware error | Use matched recall when comparing throughput or latency. |
| Query speed | QPS, p50/p95/p99 latency, batch QPS, single-query latency | Batch mode and online latency should be reported separately. |
| Preprocessing | build time, index memory, construction memory peak | Graph methods may dominate high recall but have expensive builds. |
| Updates | insertion throughput, deletion throughput, repair/rebuild cost, recall drift | Static ANN-Benchmarks-style results do not measure freshness. |
| Graph structure | average degree, graph quality, path length, connectivity, candidate set size | Graph quality alone does not imply better search performance. |
| Memory tiers | DRAM footprint, GPU HBM usage, SSD I/O, PCIe transfer, cache miss rate, cost/QPS | Required for SSD/GPU/CXL/disaggregated-memory claims. |

## Practical Rules

- Plot Pareto frontiers at matched quality. Speed claims without recall alignment are weak.
- Report implementation identity. ANN-Benchmarks warns that benchmarking an implementation is not the same as benchmarking an abstract algorithm.
- Separate build parameters from query parameters. Large graph indexes often improve high-recall robustness but increase build time and memory.
- Avoid one-dataset conclusions. The graph survey shows that algorithm rankings can change sharply across datasets.
- Keep non-graph baselines in scope. FLANN and FALCONN are still useful for tree/auto-tuning and angular LSH comparisons even when graph methods dominate high recall.
- Do not use in-memory ANN-Benchmarks results as direct evidence for SSD, CXL, RDMA, or GPU-out-of-HBM behavior.
- For graph papers, map claims onto components: initialization, candidate acquisition, neighbor selection, seed preprocessing, connectivity, seed acquisition, and routing.
- For KNNG-based methods, separate the cost of building the approximate KNN graph from the quality of the final search graph.
- For dynamic systems, report update mix, freshness semantics, latency percentiles, and whether graph repair or rebuild work is on the critical path.

## Benchmark Coverage Needed For Future ANN Work

| Claim type | Minimum comparison surface |
|---|---|
| In-memory ANN algorithm | ANN-Benchmarks-style recall-QPS plus build time and memory; include graph and non-graph baselines when metric/dataset support permits. |
| Graph pruning/routing idea | Component-level ablation against HNSW/NSG/Vamana-style baselines. |
| KNN graph construction idea | Graph construction recall, build time, construction memory, and downstream search impact after pruning. |
| Angular/cosine LSH claim | FALCONN/hyperplane-LSH-style baselines plus graph baselines at matched recall. |
| Billion-scale SSD/second-tier system | DiskANN/SPANN plus memory footprint, I/O, and cost/QPS. |
| GPU vector search beyond HBM | BANG/RUMMY/CAGRA-like GPU baselines plus PCIe/cache behavior. |
| Streaming or fresh updates | SPFresh/FreshDiskANN-style baselines plus insert/delete throughput and recall drift. |
| Vector database or filtered search | VBASE/Milvus-style query semantics and filter-selectivity tests. |

## Open Questions

- What should be the default streaming benchmark for ANN now that ANN-Benchmarks is primarily in-memory and static?
- Can graph-component metrics from the 2021 survey be integrated with modern hardware counters such as GPU cache miss rate or SSD I/O amplification?
- Which result should be treated as the user-facing metric: average QPS, tail latency at fixed QPS, or cost/QPS at matched recall?

## Related Pages

- [ANN-Benchmarks](../entities/ann-benchmarks.md)
- [ANN-Benchmarks Source Note](../source-notes/ann-benchmarks-2018.md)
- [Graph-Based ANNS Survey 2021](../source-notes/graph-based-anns-survey-2021.md)
- [FLANN](../entities/flann.md)
- [FALCONN](../entities/falconn.md)
- [NN-Descent](../entities/nn-descent.md)
- [Diversified Proximity Graph](../entities/diversified-proximity-graph.md)
- [Approximate Nearest Neighbor Search](approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](second-tier-memory-for-vector-search.md)
