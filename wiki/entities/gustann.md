---
id: gustann
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - gpu
  - ssd
source_count: 1
sources:
  - raw/sources/papers/gustann-2025.pdf
related:
  - fusionanns
  - diskann
  - spann
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# GustANN

## Profile

GustANN is a GPU-centric, CPU-assisted, SSD-resident graph ANNS system for high-throughput billion-scale vector search. It uses GPU graph traversal, CPU-assisted selective transfer, and pivot search to raise SSD utilization and cost-effectiveness.

## Main Ideas

- Reduce per-query GPU search state so many queries can run concurrently.
- Use CPU DRAM as a selective-transfer staging point instead of reading SSD pages directly into GPU-side processing.
- Use a small GPU-resident pivot graph to avoid all queries starting from the same graph region and overloading the same SSDs.

## Position In The Vault

GustANN extends the DiskANN/Starling family toward a GPU+SSD serving architecture. It is important for the broader memory-tiered ANN literature because it makes a slower/larger tier useful through batching, selective transfer, and hardware-aware scheduling.

## Key Evidence

The paper reports 247K billion-scale searches/s with one A100 and six SSDs, up to 86.7% SSD utilization, 6.98x higher throughput than DiskANN on billion-scale datasets, and at least 2.62x better cost-effectiveness than compared systems.

## Related Pages

- [GustANN Source Note](../source-notes/gustann-2025.md)
- [FusionANNS](fusionanns.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
