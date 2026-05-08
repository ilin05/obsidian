---
id: graph-based-anns-survey-2021
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - survey
  - rng
source_count: 1
sources:
  - raw/sources/papers/graph-based-anns-survey-2021.pdf
related:
  - proximity-graph-theory-for-ann
  - ann-benchmarking-methodology
  - hnsw
  - nsg
  - nn-descent
  - gnns
  - efanna
  - fanng
  - diversified-proximity-graph
  - ngt-onng
  - satellite-system-graph
  - relative-nn-descent
  - approximate-nearest-neighbor-search
confidence: high
---

# A Comprehensive Survey and Experimental Comparison of Graph-Based Approximate Nearest Neighbor Search

## Summary

Wang et al. provide a PVLDB 2021 survey and experimental comparison of graph-based ANN algorithms. The paper classifies graph ANN methods through base graph families, decomposes algorithms into fine-grained pipeline components, and compares 13 representative algorithms under a unified evaluation framework.

## Key Contributions

- Proposes a taxonomy around four base graphs: Delaunay Graph, Relative Neighborhood Graph, KNN graph, and Minimum Spanning Tree.
- Analyzes graph ANN algorithms through seven components spanning initialization, neighbor propagation, neighbor selection, seed preprocessing, connectivity, seed acquisition, and routing.
- Evaluates representative graph ANN algorithms on eight real-world datasets and 12 synthetic datasets.
- Argues that component-level evaluation can reveal which part of an algorithm actually drives performance, beyond the headline contribution in the original paper.
- Identifies RNG-based methods as a central and still-promising branch for graph ANN.

## Algorithm Taxonomy

The survey's 13 representative algorithms are organized by base graph family:

| Family | Algorithms | Interpretation |
|---|---|---|
| DG/RNG-derived | NSW, HNSW, FANNG, NGT | Use approximate Delaunay-style navigation and/or RNG-style pruning to avoid high-dimensional degree explosion. |
| KNNG | SPTAG, KGraph, EFANNA, IEH | Start from kNN graph construction or divide-and-conquer/tree/hash initialization. |
| KNNG + RNG pruning | DPG, NSG, NSSG, Vamana | Build or approximate KNNG, then prune/select neighbors for diversified directional coverage. |
| MST | HCNNG | Uses hierarchical clustering and MST-style connectivity as the graph backbone. |

## Primary Sources Now Covered

The vault now has primary notes for several algorithms that this survey previously only covered through taxonomy:

- [NN-Descent/KGraph](nn-descent-2011.md) for approximate KNN graph construction.
- [GNNS](gnns-knn-graph-2011.md) for early random-start hill climbing on a KNN graph.
- [EFANNA](efanna-2016.md) for tree-initialized KNN graph construction and query seeding.
- [FANNG](fanng-2016.md) for occlusion/RNG-like graph pruning.
- [DPG](dpg-ann-experiments-2016.md) for diversified graph improvement plus broad empirical evaluation.
- [NGT/ONNG](ngt-onng-2018.md) for degree/path adjustment and practical graph-library optimization.

## Component Pipeline

The seven components are a useful checklist for reading any graph ANN paper:

- **C1 Initialization:** random, tree/hash-based, divide-and-conquer, or NN-Descent-style initial graph.
- **C2 Candidate neighbor acquisition:** how candidate edges are obtained before pruning.
- **C3 Neighbor selection:** distance-only versus distribution/diversification-aware pruning.
- **C4 Seed preprocessing:** static entries, centroids, tree/hash structures, or random entries.
- **C5 Connectivity:** whether the constructed graph guarantees global reachability or repairs disconnected components.
- **C6 Seed acquisition:** how a query chooses entry points.
- **C7 Routing:** greedy/BFS-style graph traversal, range search, backtracking, or guided variants.

This component view is especially useful because the paper reports cases where the claimed headline component is not necessarily the part that drives most performance.

## Empirical Lessons

- Higher graph quality does not necessarily imply better search performance; DPG can spend substantial construction time improving graph quality without proportional search gains.
- Algorithm superiority is dataset-dependent. The survey gives examples where NSG is far faster than HNSW on one dataset at recall 0.99 and far slower on another.
- RNG-based methods such as HNSW/NSG/NSSG/Vamana-style designs are a promising branch because they control degree while preserving useful routing structure.
- Connectivity and neighbor distribution are not optional details; both affect the chance that search avoids local optima.
- For limited memory, NSG/NSSG-style low out-degree can be more attractive than dense KNNG/DG variants.

## Relevance To This Vault

The survey gives a taxonomy layer for the new [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md) topic. It supports the current interpretation that HNSW, NSG, NSSG/SSG, and Vamana-style systems should be compared not only by algorithm name, but by graph family, neighbor selection, connectivity repair, and routing components.

It also motivates [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md): benchmark results should be interpreted through components, not only final recall-QPS plots.

## Limits And Open Questions

- The survey primarily focuses on main-memory graph ANN algorithms.
- It explicitly leaves hardware-aware settings such as SSD/GPU, distributed deployment, machine-learning optimizations, and structured attribute constraints as future directions.
- Because it is a survey and experimental comparison, individual claims about each algorithm should still be checked against the primary source when used in paper writing.
- The vault now covers NN-Descent/KGraph, GNNS, EFANNA, FANNG, DPG, and NGT/ONNG with primary notes. SPTAG, HCNNG, PANNG, and IEH still need better primary-source coverage.

## Related Pages

- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
- [HNSW](../entities/hnsw.md)
- [NSG](../entities/nsg.md)
- [NN-Descent](../entities/nn-descent.md)
- [EFANNA](../entities/efanna.md)
- [FANNG](../entities/fanng.md)
- [Diversified Proximity Graph](../entities/diversified-proximity-graph.md)
- [NGT / ONNG](../entities/ngt-onng.md)
- [Satellite System Graph](../entities/satellite-system-graph.md)
- [Relative NN-Descent](../entities/relative-nn-descent.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
