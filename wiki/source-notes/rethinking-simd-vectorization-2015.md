---
id: rethinking-simd-vectorization-2015
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - databases
  - simd
  - vectorization
  - query-execution
source_count: 1
sources:
  - raw/sources/papers/rethinking-simd-vectorization-2015.pdf
related:
  - rethinking-simd-vectorization
  - simd-and-vectorization-for-ann-systems
  - compiled-vectorized-queries
confidence: high
---

# Rethinking SIMD Vectorization for In-Memory Databases

## Summary

This paper studies how to design in-memory database operators for advanced SIMD instructions, including gathers and scatters. It is not an ANN paper, but it gives useful operator-design lessons for vector search systems that combine scans, filters, hash tables, partitioning, and irregular memory access.

## Key Mechanism

- Studies vectorized selection scans, hash tables, partitioning, sorting, and joins.
- Separates horizontal vectorization from vertical vectorization: processing many independent keys across SIMD lanes rather than only comparing one key against many in-lane values.
- Shows that gathers/scatters enable new SIMD designs but also expose lane conflict, cache-line, and TLB issues.
- Uses buffering and partitioning to recover locality when raw gather/scatter access would be too irregular.

## Reported Results

- The paper reports up to order-of-magnitude speedups over scalar or previous vector designs for several operators.
- It reports concrete gains such as faster hash-table probing, partitioning, sorting, and hash joins on Xeon Phi and mainstream CPUs.

## Why It Matters

ANN systems frequently mix distance kernels with database-style operations: filters, posting lists, metadata predicates, candidate heaps, and partitioned scans. This paper helps distinguish SIMD-friendly contiguous computation from harder irregular-control and random-access cases.

## Limits And Open Questions

- The target workload is analytical query execution, not vector search.
- Xeon Phi-specific design points should be treated historically, but the gather/scatter and buffering lessons still transfer.

## Related Pages

- [Rethinking SIMD Vectorization](../entities/rethinking-simd-vectorization.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
