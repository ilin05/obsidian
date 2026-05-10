---
id: pipeann
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - ssd
  - graph-index
  - disk-based
  - io-pipeline
source_count: 1
sources:
  - raw/sources/papers/pipeann-2025.pdf
related:
  - second-tier-memory-for-vector-search
  - approximate-nearest-neighbor-search
  - diskann
confidence: high
---

# PipeANN

Low-latency on-disk graph-based ANNS by aligning best-first search with SSD I/O characteristics (Guo & Lu, OSDI 2025).

## Core Idea

Best-first search has pseudo-dependency between compute and I/O — the next batch of neighbors to read can be decided from the in-memory candidate pool without waiting for ongoing I/O. PipeSearch exploits this with asynchronous I/O + overlapped compute. Dynamic pipeline width and I/O waste bounding recover throughput.

## Key Properties

- Latency: 1.14–2.02× vs in-memory Vamana (bridges most of the gap)
- Latency: 35% of DiskANN on billion-scale
- Throughput: 1.71× vs DiskANN
- Works with Vamana and NSG graph indexes

## Related Pages

- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
- [PipeANN Source Note](../source-notes/pipeann-2025.md)
- [DiskANN](diskann.md)
