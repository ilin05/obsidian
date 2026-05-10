---
id: rethinking-simd-vectorization
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - paper
  - databases
  - simd
  - vectorization
source_count: 1
sources:
  - raw/sources/papers/rethinking-simd-vectorization-2015.pdf
related:
  - simd-and-vectorization-for-ann-systems
  - compiled-vectorized-queries
confidence: high
---

# Rethinking SIMD Vectorization

## Profile

This SIGMOD 2015 paper is a database-systems source on SIMD operator design. It is useful for ANN work because it explains how gathers, scatters, buffering, hash tables, selection scans, and partitioning behave under SIMD.

## Core Lesson

SIMD is strongest when the operator can recover locality and keep lanes useful. Random access, lane conflict, and variable per-lane progress must be handled explicitly rather than assumed away.

## Related Pages

- [Rethinking SIMD Vectorization Source Note](../source-notes/rethinking-simd-vectorization-2015.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
