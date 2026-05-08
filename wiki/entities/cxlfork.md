---
id: cxlfork
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - system
  - cxl
  - serverless
source_count: 1
sources:
  - raw/CXLfork.pdf
related:
  - cxlfork-2025
  - cxl-disaggregated-memory-systems
confidence: high
---

# CXLfork

## Profile

CXLfork is a CXL-based remote-fork interface for fast cross-node process cloning with state sharing on shared CXL memory.

## Why It Matters

- Brings memory-sharing and low-copy process cloning semantics to cluster-level serverless runtime design.
- Represents a CXL-native system-interface redesign rather than direct RDMA-era adaptation.

## Related Pages

- [CXLfork Source Note](../source-notes/cxlfork-2025.md)
- [CXL Disaggregated Memory Systems](../topics/cxl-disaggregated-memory-systems.md)

