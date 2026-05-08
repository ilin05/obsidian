---
id: diskann-2019
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - ann
  - graph-index
  - ssd
  - diskann
source_count: 1
sources:
  - raw/sources/papers/diskann-2019.pdf
related:
  - diskann
  - hnsw
  - nsg
  - approximate-nearest-neighbor-search
confidence: high
---

# DiskANN: Fast Accurate Billion-point Nearest Neighbor Search on a Single Node

## Summary

DiskANN is an SSD-resident ANN system centered on a new graph construction algorithm (Vamana). Its core claim is that high-recall, low-latency, billion-scale ANN serving can be achieved on a single node with limited RAM by careful graph/index design.

## Key Contributions

- Introduces Vamana, a graph index construction method designed to reduce hops and support SSD-resident search efficiently.
- Combines graph traversal with SSD layout/caching strategy to reduce expensive random I/O round trips.
- Shows how to combine graph index with product quantization to keep memory footprint low while preserving high recall.

## Reported Results

- On SIFT1B, reports >5000 QPS, <3 ms mean latency, and 95%+ 1-recall@1 on a 16-core machine with 64 GB RAM plus SSD.
- Claims better high-recall density than compared HNSW/NSG configurations under similar memory constraints.

## Limits And Open Questions

- Performance depends on SSD behavior and beam-search/system tuning choices.
- Later disaggregated-memory work asks whether SSD-centered assumptions remain optimal as memory hierarchy changes.

## Related Pages

- [DiskANN](../entities/diskann.md)
- [HNSW](../entities/hnsw.md)
- [NSG](../entities/nsg.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
