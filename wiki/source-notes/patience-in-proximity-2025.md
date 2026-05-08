---
id: patience-in-proximity-2025
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - ann
  - hnsw
  - early-termination
  - dense-retrieval
source_count: 1
sources:
  - raw/patience.pdf
related:
  - patience-in-proximity
  - hnsw
  - approximate-nearest-neighbor-search
confidence: medium
---

# Patience in Proximity: A Simple Early Termination Strategy for HNSW Graph Traversal in Approximate k-NN Search

## Summary

This paper proposes a lightweight early-termination strategy for HNSW traversal, called "Patience in Proximity", using saturation-based stopping during graph exploration to cut compute without large quality loss.

## Key Contributions

- Introduces a simple dynamic thresholding rule to halt HNSW traversal earlier.
- Adapts a "patience" style termination idea from clustering-informed ANN to HNSW graph search.
- Evaluates the efficiency-effectiveness trade-off on BEIR benchmark datasets.

## Reported Results

- Reports computational savings from earlier stopping with limited impact on retrieval quality in the paper's evaluated settings.

## Limits And Open Questions

- This is a short paper focused on a specific traversal heuristic; broad generalization across all HNSW implementations needs further validation.
- Optimal patience thresholds likely remain dataset- and workload-dependent.

## Related Pages

- [Patience in Proximity](../entities/patience-in-proximity.md)
- [HNSW](../entities/hnsw.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)

