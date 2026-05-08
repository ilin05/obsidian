---
id: diskann
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - system
  - ann
  - ssd
source_count: 1
sources:
  - raw/DiskANN.pdf
related:
  - diskann-2019
  - hnsw
  - nsg
  - second-tier-memory-for-vector-search
  - approximate-nearest-neighbor-search
confidence: high
---

# DiskANN

## Profile

DiskANN is an SSD-resident billion-scale ANN system that combines the Vamana graph index with storage-aware search layout and caching.

## Why It Matters

- It is a reference system for high-recall ANN under constrained DRAM budgets.
- It anchors the SSD-centered design point in this vault's ANN systems branch.

## Related Pages

- [DiskANN Source Note](../source-notes/diskann-2019.md)
- [HNSW](hnsw.md)
- [NSG](nsg.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)

