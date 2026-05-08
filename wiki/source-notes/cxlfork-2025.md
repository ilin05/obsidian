---
id: cxlfork-2025
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - cxl
  - disaggregated-memory
  - serverless
  - systems
source_count: 1
sources:
  - raw/CXLfork.pdf
related:
  - cxlfork
  - cxl-disaggregated-memory-systems
confidence: high
---

# CXLfork: Fast Remote Fork over CXL Fabrics

## Summary

This paper proposes CXLfork, a remote-fork interface over shared CXL memory for near zero-serialization and near zero-copy inter-node process cloning.

## Key Contributions

- Uses globally shared CXL memory to checkpoint and share process state across nodes.
- Reduces serialization and transfer overhead compared with file/RDMA-oriented remote-fork paths.
- Adds state-tiering control between local memory and CXL memory to balance latency and memory savings.
- Demonstrates a serverless autoscaling design (CXLporter) built on top of CXLfork.

## Reported Results

- Reports restore latency close to local fork.
- Reports average speedup over state-of-practice remote-fork baselines and major local-memory reduction in evaluated serverless scenarios.

## Limits And Open Questions

- Benefits depend on CXL fabric characteristics and integration depth with OS/runtime.
- Generality beyond serverless-style cloning workloads requires further validation.

## Related Pages

- [CXLfork](../entities/cxlfork.md)
- [CXL Disaggregated Memory Systems](../topics/cxl-disaggregated-memory-systems.md)

