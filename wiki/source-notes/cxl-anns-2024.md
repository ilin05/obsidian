---
id: cxl-anns-2024
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - ann
  - disaggregated-memory
  - cxl
  - systems
source_count: 1
sources:
  - raw/sources/papers/cxl-anns-2024.pdf
related:
  - cxl-anns
  - disaggregated-memory-vector-search
  - approximate-nearest-neighbor-search
confidence: high
---

# Bridging Software-Hardware for CXL Memory Disaggregation in Billion-Scale Nearest Neighbor Search

## Summary

This paper presents CXL-ANNS, a software-hardware co-design for billion-scale ANN search on CXL-disaggregated memory. It argues that simply placing ANN graphs in CXL memory is not enough because far-memory characteristics can hurt latency.

## Key Contributions

- Stores billion-point graph datasets in a CXL memory pool to avoid capacity limits of local DRAM.
- Uses node-relationship-aware local caching and ANN-aware prefetching for uncached nodes.
- Exploits CXL network topology and parallel hardware execution to relax dependency bottlenecks in neighbor search.

## Reported Results

- Reports 93.3% lower query latency than tested state-of-the-art platforms.
- Reports 68.0% lower latency than an oracle-style system with unlimited local DRAM capacity in the paper's setup.

## Limits And Open Questions

- This is a platform co-design result; portability to other hardware fabrics requires careful validation.
- The balance between software complexity and hardware specialization remains a practical deployment question.

## Related Pages

- [CXL-ANNS](../entities/cxl-anns.md)
- [Disaggregated Memory Vector Search](../topics/disaggregated-memory-vector-search.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
