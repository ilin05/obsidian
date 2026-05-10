---
id: pipeann-2025
type: source-note
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

# Achieving Low-Latency Graph-Based Vector Search via Aligning Best-First Search Algorithm with SSD

Hao Guo and Youyou Lu, OSDI 2025 (Tsinghua University).

## Summary

PipeANN bridges the latency gap between in-memory and on-disk graph-based ANNS by fundamentally rethinking the best-first search algorithm for SSD characteristics. The key observation: compute and I/O in best-first search have a **pseudo-dependency** — the next batch of neighbors to read can be decided from the in-memory candidate pool without waiting for ongoing I/O or compute to finish. PipeSearch exploits this by issuing asynchronous I/O for the current best candidates regardless of pending operations, overlapping compute with I/O. PipeANN adds dynamic pipeline width adjustment and I/O waste control to recover throughput. On billion-scale datasets, PipeANN achieves 1.14–2.02× the latency of in-memory Vamana and only 35% of DiskANN's latency.

## Key Contributions

- **Identifies the intrinsic mismatch** between best-first search (ordered compute-I/O across steps, synchronous batch reads) and SSD characteristics (long I/O latency, parallel asynchronous I/O capability).
- **PipeSearch algorithm**: Asynchronously reads the current nearest unexplored neighbors whenever the I/O pipeline has capacity, regardless of ongoing I/O or unfinished compute. Overlaps neighbor exploration with I/O.
- **Dynamic pipeline width**: Starts search with narrow pipeline (high throughput, less waste) and widens it as more top-k candidates accumulate (more tolerance for I/O waste).
- **I/O waste bounding**: Limits the number of missed neighbors (ongoing I/O + read-but-unexplored) when issuing new I/O, preventing sub-optimal I/O decisions.

## Algorithm Details

### PipeSearch (vs Best-First)
```
Best-First:                    PipeSearch:
  Read batch → Explore →         Issue async reads →
  Read batch → Explore →         While I/O pending:
  ...                              Explore completed reads →
                                   Issue new reads (if pipeline not full) →
                                 ...
```

### Dynamic Pipeline Width
1. Early search: narrow pipeline (little I/O waste, fewer top-k candidates identified yet).
2. Late search: wide pipeline (more unexplored top-k neighbors in candidate pool → I/O waste is lower because reads are more likely to be useful).

### Anti-Accumulation Policy
- When multiple I/Os complete simultaneously, explore one neighbor and issue one I/O at a time (rather than flooding the pipeline). This prevents read-but-unexplored neighbor accumulation.

## Reported Results

- Latency: 1.14–2.02× vs in-memory Vamana (bridges most of the gap)
- Latency: 35.0% of DiskANN's latency on billion-scale (Deep1B, SIFT1B)
- Throughput: 1.71× vs DiskANN at 0.9 recall
- Works with various graph indexes (Vamana, NSG)
- SSD IOPS utilization: significantly improved over best-first

## Significance

PipeANN is the first work to systematically challenge the assumption that best-first search is optimal for on-disk graph ANNS. By recognizing that graph indexes have multiple search paths (not a single one like B+-trees), it opens the design space for I/O scheduling algorithms that exploit the inherent redundancy in graph connectivity. The result is a practical system that narrows the SSD-DRAM latency gap to nearly 1×.

## Limits And Open Questions

- Higher throughput comes at some latency cost vs pure PipeSearch (trade-off managed by dynamic pipeline).
- Speculative I/O can still cause waste for queries that converge quickly on very small candidate sets.
- Assumes NVMe SSD characteristics; results may differ on other storage media.
- Not evaluated with concurrent query workloads where SSD bandwidth contention could change the dynamics.

## Related Pages

- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [DiskANN](../entities/diskann.md)
