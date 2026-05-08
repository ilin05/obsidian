---
id: nsg
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - method
  - ann
  - graph-index
source_count: 1
sources:
  - raw/sources/papers/nsg-2019.pdf
related:
  - nsg-2019
  - hnsw
  - diskann
  - approximate-nearest-neighbor-search
confidence: high
---

# NSG

## Profile

Navigating Spreading-out Graph (NSG) is a practical graph index for ANN search, designed as an approximation to MRNG with strong focus on path quality, connectivity, and memory efficiency.

## Why It Matters

- It pushed graph-based ANN to stronger billion-scale practicality and was reported in production use.
- It serves as a key comparison point for later graph systems such as DiskANN.

## Related Pages

- [NSG Source Note](../source-notes/nsg-2019.md)
- [HNSW](hnsw.md)
- [DiskANN](diskann.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
