---
id: dpg-ann-experiments-2016
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - benchmarking
  - graph-index
  - pruning
source_count: 1
sources:
  - raw/sources/papers/dpg-ann-experiments-2016.pdf
related:
  - diversified-proximity-graph
  - ann-benchmarking-methodology
  - nn-descent
  - nsg
  - proximity-graph-theory-for-ann
confidence: high
---

# Approximate Nearest Neighbor Search on High Dimensional Data - Experiments, Analyses, and Improvement

## Summary

Li, Zhang, Sun, Wang, Zhang, and Lin present a broad empirical study of approximate nearest-neighbor search algorithms and propose Diversified Proximity Graph (DPG) as an empirical graph-index improvement. The paper compares methods across algorithm families, datasets, metrics, query workloads, and tuning behavior.

## Key Contributions

- Evaluates 16 ANNS algorithms across 20 datasets, spanning LSH-based, space-partitioning, encoding/hash, and neighborhood-graph methods.
- Reports more than recall/QPS: search quality, index size, scalability, robustness, query workload effects, updateability, and tuning effort are all considered.
- Disables hardware-specific optimizations to make algorithmic comparison more controlled.
- Identifies discrepancies across prior benchmark studies and attributes some differences to datasets, tuning, and implementation choices.
- Proposes DPG, a diversified graph method intended to improve high-recall query efficiency over plain kNN graph traversal.

## Why It Matters

This paper bridges [ANN-Benchmarks](ann-benchmarks-2018.md) and the graph survey. ANN-Benchmarks emphasizes implementation-level Pareto frontiers; DPG's study emphasizes cross-family empirical diagnosis and introduces a graph diversification step that later sits between KGraph/NN-Descent and NSG/NSSG/Vamana-style pruning.

## Limits And Open Questions

- This source is the 2016 arXiv v1.0; publication metadata and later DPG references should be checked before final citation.
- The DPG method is empirical and should be separated from the paper's broader benchmarking study.
- Later graph survey results suggest that higher graph quality does not always translate into proportional search gains, so DPG should be read as one step in the pruning lineage rather than a final answer.

## Related Pages

- [Diversified Proximity Graph](../entities/diversified-proximity-graph.md)
- [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md)
- [NN-Descent](../entities/nn-descent.md)
- [NSG](../entities/nsg.md)
