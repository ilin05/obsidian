---
id: cxl-disaggregated-memory-systems
type: topic
status: active
created: 2026-04-08
updated: 2026-05-08
tags:
  - cxl
  - disaggregated-memory
  - systems
source_count: 6
sources:
  - raw/sources/papers/cxlfork-2025.pdf
  - raw/sources/papers/polarcxlmem-2025.pdf
  - raw/sources/papers/cxl-anns-2024.pdf
  - raw/sources/papers/cxl-memory-characterization-2023.pdf
  - vldb-paper-outline.md
  - vldb-paper-intro-draft.md
related:
  - cxlfork
  - polarcxlmem
  - cxl-anns
  - cxl-vector
  - cxl-vector-commodity-cxl-hnsw-serving
  - disaggregated-memory-vector-search
confidence: medium
---

# CXL Disaggregated Memory Systems

## Summary

This topic tracks CXL-native system redesigns where shared or disaggregated memory is treated as an execution substrate rather than a storage-like remote tier.

## Current View

- **Hardware grounding:** The [CXL memory characterization paper](../source-notes/cxl-memory-characterization-2023.md) provides the true-CXL empirical baseline: 29-41 ns access latency, competitive sequential bandwidth, and distinct cache-hierarchy behavior.
- [CXLfork](../entities/cxlfork.md) emphasizes cross-node process cloning and state sharing for serverless runtimes.
- [PolarCXLMem](../entities/polarcxlmem.md) emphasizes database buffer/recovery/coherence redesign in cloud-native DB systems.
- [CXL-ANNS](../entities/cxl-anns.md) emphasizes CXL ANN with hardware/software co-design.
- [CXL-Vector](../entities/cxl-vector.md) now occupies the commodity software point in the design space: HNSW serving on CXL memory without endpoint compute, using DRAM-local routing and scheduled rerank access.

## Hardware Reality

| Property | Finding |
|---|---|
| True CXL latency | 29-41 ns on true CXL hardware, not emulated-CXL latency |
| Bandwidth behavior | Competitive for sequential access, sensitive to cache coherence and random pressure |
| LLC interaction | CXL pages interact with LLC differently from local DDR |
| Latency-sensitive workloads | Tail latency can degrade under random access |
| Bandwidth-intensive workloads | Can benefit from expanded memory bandwidth/capacity |

## CXL-Vector Implication

CXL-Vector's current paper claim depends on treating CXL as neither DRAM nor SSD. Its raw-vector stage is byte-addressable and much faster than SSD, but still bandwidth-constrained under many concurrent HNSW queries. That is why the latest framing emphasizes scheduling and bounded rerank.

## Open Questions

- How stable are current wins across CXL controllers and switch fabrics?
- Which CXL abstractions are reusable across ANN, databases, and serverless runtimes?
- Can CXL-Vector's rerank scheduling ideas transfer to other CXL-native workloads that have a bounded expensive stage?

## Related Pages

- [CXLfork](../entities/cxlfork.md) · [PolarCXLMem](../entities/polarcxlmem.md) · [CXL-ANNS](../entities/cxl-anns.md) · [CXL-Vector](../entities/cxl-vector.md)
- [Commodity CXL HNSW Serving](cxl-vector-commodity-cxl-hnsw-serving.md)
- [Disaggregated Memory Vector Search](disaggregated-memory-vector-search.md)
- [CXLfork Source Note](../source-notes/cxlfork-2025.md)
- [PolarCXLMem Source Note](../source-notes/polarcxlmem-2025.md)
- [CXL-ANNS Source Note](../source-notes/cxl-anns-2024.md)
- [CXL Memory Characterization Source Note](../source-notes/cxl-memory-characterization-2023.md)

