---
id: compiled-vectorized-queries-2018
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vectorization
  - query-execution
  - database-systems
  - simd
  - compilation
source_count: 1
sources:
  - raw/sources/papers/compiled-vectorized-queries-2018.pdf
related:
  - compiled-vectorized-queries
  - simd-investments
  - fastlanes
  - simd-and-vectorization-for-ann-systems
  - milvus
  - morsel-driven-parallelism
confidence: high
---

# Everything You Always Wanted to Know About Compiled and Vectorized Queries But Were Afraid to Ask

## Bibliographic Note

Timo Kersten, Viktor Leis, Alfons Kemper, Thomas Neumann, Andrew Pavlo, and Peter Boncz. 2018. *Everything You Always Wanted to Know About Compiled and Vectorized Queries But Were Afraid to Ask*. PVLDB 11(13), 2209-2222. DOI: `10.14778/3275366.3275370`.

The related entity page is [Compiled and Vectorized Queries](../entities/compiled-vectorized-queries.md).

## Core Problem

The paper compares two major database execution paradigms under a controlled implementation: vectorized execution and data-centric compiled execution. Existing systems differed in too many other design choices, so prior comparisons were hard to interpret.

## Main Findings

- Both execution models are efficient but have different strengths.
- Vectorized execution tends to hide cache-miss latency better, especially in join-heavy workloads.
- Data-centric compiled execution often executes fewer CPU instructions, which helps computation-heavy and cache-resident workloads.
- SIMD can produce large microbenchmark speedups, but the end-to-end benefit is limited when memory latency dominates.
- Vectorized engines are structurally well-positioned to use SIMD because primitives operate over batches.
- Morsel-driven parallelism can be applied to both models, reducing the model-level difference when parallelism is implemented carefully.

## SIMD-Specific Lessons

The paper's SIMD section is directly relevant to ANN system benchmarking:

- AVX-512 can make some selection microbenchmarks much faster.
- Sparse loading through selection vectors reduces gains.
- Hash-table probing receives less SIMD benefit when working sets exceed cache and memory latency dominates.
- Compiler auto-vectorization helps but does not fully replace manual SIMD optimization.
- The paper notes that SIMD gains can be larger when data is compressed.

## Relevance To ANNS Systems

ANN systems mix regular and irregular work. Dense vector distance computation and PQ scans resemble vectorized primitives. Graph traversal and hash/metadata lookup resemble cache-miss-heavy pointer chasing.

This paper suggests that ANN evaluation should not only ask "is SIMD used?" but also where SIMD appears in the full query path and whether it affects the end-to-end bottleneck.

## Follow-up Questions

- For CPU ANN libraries, which query stages are compute-bound enough for SIMD to matter?
- Should ANN papers report cache misses and instruction counts when claiming CPU optimizations?
- Can vectorized execution ideas help batch multiple ANN queries without harming tail latency?
