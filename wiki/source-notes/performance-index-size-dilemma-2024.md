---
id: performance-index-size-dilemma-2024
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - ann
  - systems
  - index-size
  - second-tier-memory
source_count: 1
sources:
  - raw/Characterizing the Dilemma of Performance and Index Size in Billion-Scale Vector Search .pdf
related:
  - second-tier-memory-for-vector-search
  - disaggregated-memory-vector-search
  - diskann
  - cxl-anns
  - d-hnsw
  - approximate-nearest-neighbor-search
confidence: high
---

# Characterizing the Dilemma of Performance and Index Size in Billion-Scale Vector Search and Breaking It with Second-Tier Memory

## Summary

This paper characterizes a core systems trade-off in SSD-based billion-scale vector search: improving throughput often requires large index amplification. It argues that second-tier memory (e.g., RDMA/CXL/NVM-attached memory) can break this dilemma when index layout and execution are redesigned for fine-grained access.

## Key Contributions

- Quantifies trade-off in SSD-based graph and cluster indexes between throughput and index amplification.
- Identifies mismatch between SSD coarse-grained access and fine-grained ANN index access patterns.
- Proposes second-tier-memory-centered graph/cluster index redesigns with pipeline/layout changes.
- Provides an end-to-end comparison of graph vs cluster behavior across SSD, RDMA, CXL, and NVM settings.

## Reported Results

- Reports that pushing throughput in existing SSD indexes can require large amplification (paper example: 5.8x and 7.7x).
- Reports substantial amplification reductions with improved second-tier-memory designs while maintaining or improving performance.

## Limits And Open Questions

- Results depend on the tested hardware stack and software implementation details.
- The paper's conclusions are system-level and may require workload-specific validation for production deployment.

## Related Pages

- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
- [DiskANN](../entities/diskann.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
