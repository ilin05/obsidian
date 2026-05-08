---
id: polarcxlmem-2025
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - cxl
  - disaggregated-memory
  - cloud-database
  - systems
source_count: 2
sources:
  - raw/Unlocking the Potential of CXL for Disaggregated Memory in CloudNative Databases.pdf
  - raw/Unlocking the Potential of CXL for Disaggregated Memory in Cloud-Native Databases.md
related:
  - polarcxlmem
  - cxl-disaggregated-memory-systems
confidence: high
---

# Unlocking the Potential of CXL for Disaggregated Memory in Cloud-Native Databases

## Summary

This paper presents PolarCXLMem, a CXL-switch-based disaggregated-memory system for cloud-native databases, and proposes PolarRecv for fast crash recovery with CXL-resident buffer state reuse.

## Key Contributions

- Designs a CXL-switch-based memory disaggregation path for database buffer pools.
- Proposes instant-recovery workflow (PolarRecv) that reuses CXL-resident buffer state after crashes.
- Proposes coherence/data-sharing support for multi-primary database deployment over CXL memory.
- Provides end-to-end evaluation in PolarDB deployment context.

## Reported Results

- Reports up to 2.1x throughput gain in pooling scenarios versus evaluated RDMA-based systems.
- Reports up to 1.55x throughput gain in sharing scenarios in the paper's evaluated setup.

## Limits And Open Questions

- Results are tightly coupled to specific hardware/software stack and deployment assumptions.
- Cross-system comparability with ANN-focused CXL work is indirect and mostly architectural.

## Related Pages

- [PolarCXLMem](../entities/polarcxlmem.md)
- [CXL Disaggregated Memory Systems](../topics/cxl-disaggregated-memory-systems.md)

