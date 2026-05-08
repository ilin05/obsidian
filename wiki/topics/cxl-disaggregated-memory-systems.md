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
source_count: 4
sources:
  - raw/sources/papers/cxlfork-2025.pdf
  - raw/sources/papers/polarcxlmem-2025.pdf
  - raw/sources/papers/cxl-anns-2024.pdf
  - raw/sources/papers/cxl-memory-characterization-2023.pdf
related:
  - cxlfork
  - polarcxlmem
  - cxl-anns
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

## Hardware Reality

| Property | Finding |
|---|---|
| True CXL latency | 29-41 ns on true CXL hardware, not emulated-CXL latency |
| Bandwidth behavior | Competitive for sequential access, sensitive to cache coherence and random pressure |
| LLC interaction | CXL pages interact with LLC differently from local DDR |
| Latency-sensitive workloads | Tail latency can degrade under random access |
| Bandwidth-intensive workloads | Can benefit from expanded memory bandwidth/capacity |

## Open Questions

- How stable are current wins across CXL controllers and switch fabrics?
- Which CXL abstractions are reusable across ANN, databases, and serverless runtimes?

## Related Pages

- [CXLfork](../entities/cxlfork.md) · [PolarCXLMem](../entities/polarcxlmem.md) · [CXL-ANNS](../entities/cxl-anns.md)
- [Disaggregated Memory Vector Search](disaggregated-memory-vector-search.md)
- [CXLfork Source Note](../source-notes/cxlfork-2025.md)
- [PolarCXLMem Source Note](../source-notes/polarcxlmem-2025.md)
- [CXL-ANNS Source Note](../source-notes/cxl-anns-2024.md)
- [CXL Memory Characterization Source Note](../source-notes/cxl-memory-characterization-2023.md)
