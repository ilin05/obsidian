---
id: rummy
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - gpu
  - host-memory
source_count: 1
sources:
  - raw/sources/papers/rummy-2024.pdf
related:
  - bang
  - gustann
  - fusionanns
  - faiss
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# RUMMY

## Profile

RUMMY is an NSDI 2024 GPU-accelerated vector query processing system for datasets larger than GPU memory. It extends a FAISS IVF-style workflow with reordered pipelining between host memory and GPU memory.

## Main Ideas

- Retrofit batch query plans around clusters to avoid redundant cluster transfers.
- Balance clusters and use dynamic kernel padding to improve GPU SM utilization.
- Reorder and group query work at runtime to overlap host-to-GPU transfer with computation.
- Treat GPU memory management as part of vector-query scheduling.

## Position In The Vault

RUMMY is a host-memory-plus-GPU design point. It should be used as a pipeline-scheduling baseline when comparing systems that move candidate data across CPU memory, GPU HBM, SSD, or other second-tier memory.

## Key Evidence

The paper reports up to 135x over IVF with CUDA unified memory and up to 23.1x over CPU IVF with 64 vCPUs, with up to 37.7x better per-dollar performance.

## Related Pages

- [RUMMY Source Note](../source-notes/rummy-2024.md)
- [BANG](bang.md)
- [GustANN](gustann.md)
- [FusionANNS](fusionanns.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
