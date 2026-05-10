---
id: compiled-vectorized-queries
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vectorization
  - query-execution
  - database-systems
  - simd
source_count: 1
sources:
  - raw/sources/papers/compiled-vectorized-queries-2018.pdf
related:
  - simd-and-vectorization-for-ann-systems
  - simd-investments
  - fastlanes
  - morsel-driven-parallelism
confidence: high
---

# Compiled and Vectorized Queries

## Profile

This page represents the PVLDB 2018 paper *Everything You Always Wanted to Know About Compiled and Vectorized Queries But Were Afraid to Ask*. The paper compares vectorized query execution and data-centric compiled execution in a controlled experimental system.

## Main Ideas

- Vectorized execution batches tuples and tends to hide cache-miss latency better.
- Data-centric compiled execution fuses operators and often executes fewer instructions.
- SIMD microbenchmark wins do not always translate to end-to-end query wins.
- Memory latency, sparse access, and selection vectors can reduce SIMD benefit.

## Position In The Vault

This is a foundational execution-model reference for ANN systems. ANN pipelines combine dense vector arithmetic with irregular index traversal, so the paper helps separate computation-bound SIMD opportunities from memory-bound or control-flow-bound stages.

## Related Pages

- [Source Note](../source-notes/compiled-vectorized-queries-2018.md)
- [SIMD and Vectorization for ANN Systems](../topics/simd-and-vectorization-for-ann-systems.md)
- [SIMD Investments](simd-investments.md)
- [Morsel-driven Parallelism](morsel-driven-parallelism.md)
