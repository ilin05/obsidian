---
id: hm-ann
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - heterogeneous-memory
  - graph-index
  - billion-scale
source_count: 1
sources:
  - raw/sources/papers/hm-ann-2020.pdf
related:
  - second-tier-memory-for-vector-search
  - approximate-nearest-neighbor-search
  - hnsw
confidence: high
---

# HM-ANN

Heterogeneous memory graph-based ANNS for billion-scale search without vector compression (Ren et al., NeurIPS 2020).

## Core Idea

Generalize HNSW for heterogeneous memory (DRAM + Optane PMM): top-down insertion builds L0 (all vectors) in slow memory; bottom-up promotion elevates pivots to upper layers in fast memory. Most search accesses hit fast memory; full-precision vectors in slow memory eliminate compression errors. Dynamic migration prefetches likely-to-be-accessed nodes.

## Key Properties

- 95% top-1 recall in <1ms on billion-scale
- 46% higher recall than compression-based methods at matched latency
- 2–5.8× faster than HNSW/NSG baselines on HM
- No vector compression — full-precision distance computation
- PMM-dependent; CXL memory is the natural successor target

## Related Pages

- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
- [HM-ANN Source Note](../source-notes/hm-ann-2020.md)
- [HNSW](hnsw.md)
- [NSG](nsg.md)
