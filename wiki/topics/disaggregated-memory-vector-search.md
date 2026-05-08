---
id: disaggregated-memory-vector-search
type: topic
status: active
created: 2026-04-08
updated: 2026-05-08
tags:
  - ann
  - systems
  - disaggregated-memory
  - cxl
  - rdma
source_count: 6
sources:
  - raw/sources/papers/cxl-anns-2024.pdf
  - raw/sources/papers/d-hnsw-2025.pdf
  - raw/sources/papers/performance-index-size-dilemma-2024.pdf
  - raw/sources/papers/cxl-memory-characterization-2023.pdf
  - vldb-paper-outline.md
  - vldb-paper-intro-draft.md
related:
  - cxl-anns
  - d-hnsw
  - cxl-vector
  - cxl-vector-commodity-cxl-hnsw-serving
  - cross-query-rerank-coalescing
  - cxl-disaggregated-memory-systems
  - second-tier-memory-for-vector-search
  - approximate-nearest-neighbor-search
confidence: medium
---

# Disaggregated Memory Vector Search

## Summary

This topic covers ANN systems that move beyond local-DRAM-only design and explicitly optimize for disaggregated or second-tier memory access patterns.

## Current View

- Naively porting ANN indexes to far memory underperforms because graph traversal issues many small, irregular accesses.
- Strong designs combine index-layout changes, caching or prefetching, and execution-pipeline restructuring.
- Hardware-aware coordination remains first-class, but the hardware assumption matters: CXL-ANNS assumes custom endpoint acceleration, d-HNSW assumes RDMA-scale disaggregation, and CXL-Vector assumes commodity CXL memory with no device-side compute.
- The latest [CXL-Vector](../entities/cxl-vector.md) paper framing emphasizes PCIe bandwidth as a scheduled resource: raw vectors are confined to bounded rerank, and batch execution overlaps remaining CXL access with other query work.

## Design Axes

| Axis | CXL-ANNS | d-HNSW | CXL-Vector |
|---|---|---|---|
| Memory fabric | CXL | RDMA/disaggregated memory | commodity CXL |
| Main bottleneck | device/host coordination | network round trips | PCIe/CXL bandwidth under batched HNSW |
| Computation | hardware/software co-design | host-side traversal adaptations | pure software, SQ8/VNNI, bounded rerank |
| Current vault role | external related work | external related work | active paper project |

## Open Questions

- Which design principles transfer cleanly across CXL, RDMA, and other memory fabrics?
- How much of measured gain comes from algorithmic changes versus hardware-specific tuning?
- Can cross-query rerank coalescing become a general CXL-memory pattern beyond HNSW?

## Related Pages

- [CXL-ANNS](../entities/cxl-anns.md)
- [d-HNSW](../entities/d-hnsw.md)
- [CXL-Vector](../entities/cxl-vector.md)
- [Commodity CXL HNSW Serving](cxl-vector-commodity-cxl-hnsw-serving.md)
- [Cross-Query Rerank Coalescing](cross-query-rerank-coalescing.md)
- [CXL Disaggregated Memory Systems](cxl-disaggregated-memory-systems.md)
- [Second-tier Memory for Vector Search](second-tier-memory-for-vector-search.md)
- [Approximate Nearest Neighbor Search](approximate-nearest-neighbor-search.md)

