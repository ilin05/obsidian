---
id: smartanns
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - smartssd
  - near-data-processing
source_count: 1
sources:
  - raw/sources/papers/smartanns-2024.pdf
related:
  - spann
  - diskann
  - ansmet
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# SmartANNS

## Profile

SmartANNS is a USENIX ATC 2024 billion-point ANNS system using commercial Samsung SmartSSDs. It combines host-side shard coordination with FPGA-backed HNSW search on SmartSSDs.

## Main Ideas

- Keep shard centroids in host memory and shard-level HNSW indexes on SmartSSDs.
- Use host CPUs as global coordinators to reduce SmartSSD search scope.
- Balance load and data reuse across multiple SmartSSDs with task scheduling.
- Use learning-based shard pruning to avoid unnecessary near-data computations.

## Position In The Vault

SmartANNS is the main SmartSSD/NDP comparison point. It differs from commodity SSD systems because search computation happens inside storage devices rather than on host CPU or GPU after data movement.

## Key Evidence

At 90% Recall@10, the paper reports 8.5x-10.7x QPS over CSDANNS and 3.76x scaling from one to four SmartSSDs on SIFT1B.

## Related Pages

- [SmartANNS Source Note](../source-notes/smartanns-2024.md)
- [SPANN](spann.md)
- [DiskANN](diskann.md)
- [ANSMET](ansmet.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
