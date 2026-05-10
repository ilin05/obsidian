---
id: simd-investments
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - simd
  - vectorization
  - query-execution
source_count: 1
sources:
  - raw/sources/papers/simd-investments-2020.pdf
related:
  - simd-and-vectorization-for-ann-systems
  - compiled-vectorized-queries
confidence: high
---

# SIMD Investments

## Profile

This page represents the VLDB Journal 2020 paper *Make the most out of your SIMD investments*. The paper studies AVX-512 control-flow divergence in compiled query pipelines and proposes lane refill strategies.

## Main Ideas

- SIMD lanes become underutilized when different tuples take different control-flow paths.
- AVX-512 masks, compress/expand, and permute operations can refill inactive lanes.
- Refill must be integrated into the query pipeline without unnecessary materialization.
- The best strategy depends on operator cost, refill overhead, and how far divergence occurs from the pipeline source.

## Position In The Vault

This paper is a CPU execution-layer reference for ANN systems with irregular candidate filtering or graph traversal. It is especially useful when reasoning about why SIMD speedups may collapse under divergence.

## Related Pages

- [Source Note](../source-notes/simd-investments-2020.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
- [Compiled and Vectorized Queries](compiled-vectorized-queries.md)
