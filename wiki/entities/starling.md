---
id: starling
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - ssd
  - graph-index
source_count: 1
sources:
  - raw/sources/papers/starling-2024.pdf
related:
  - diskann
  - spann
  - hnsw
  - nsg
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# Starling

## Profile

Starling is a disk-resident graph-index framework for vector database data segments. It optimizes disk graph layout and search strategy under per-segment memory and disk constraints.

## Main Ideas

- Use an in-memory navigation graph to find query-aware disk-graph entry points.
- Reorder the disk graph layout so likely neighboring vertices share disk blocks.
- Search by blocks rather than isolated vertices to improve useful work per disk I/O.
- Apply the framework to graph algorithms such as Vamana, NSG, and HNSW.

## Position In The Vault

Starling extends the DiskANN branch by focusing on segment-local graph layout and block utilization. It is relevant whenever a vector database organizes billion-scale data into segments rather than one monolithic index.

## Key Evidence

The paper reports 43.9x higher throughput and 98% lower query latency than state-of-the-art methods under the same accuracy in its segment setting, and more than 2x over DiskANN on one billion BIGANN in the high-recall regime.

## Related Pages

- [Starling Source Note](../source-notes/starling-2024.md)
- [DiskANN](diskann.md)
- [SPANN](spann.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
