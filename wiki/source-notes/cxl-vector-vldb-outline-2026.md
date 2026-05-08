---
id: cxl-vector-vldb-outline-2026
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - cxl-vector
  - vldb
  - outline
source_count: 1
sources:
  - vldb-paper-outline.md
related:
  - cxl-vector-vldb-paper
  - cxl-vector
  - cxl-vector-commodity-cxl-hnsw-serving
confidence: high
---

# CXL-Vector VLDB Outline

## Source

`vldb-paper-outline.md`

## Summary

The outline reframes CXL-Vector as a 12-page PVLDB/VLDB paper. Its central argument is that commodity CXL creates a new serving tier between DRAM and SSD, but naive HNSW placement on CXL collapses under multi-threaded PCIe bandwidth pressure.

## Core Structure

- Abstract: capacity-bound vector search, commodity CXL, three failure modes, three system components, headline results.
- Introduction: memory wall, CXL opportunity, naive CXL-HNSW collapse, scheduling insight, contributions.
- Background: HNSW stages, CXL memory, SQ8, morsel-driven parallelism.
- Motivation: naive placement, single-query versus batched execution, FAISS-HNSW-SQ8 design space.
- System: layered placement, coarse-to-exact search, morsel-driven execution.
- Evaluation: main results, bottleneck decomposition, scaling, sensitivity, memory footprint, build cost, NUMA verification.

## Key Claims

- CXL-Vector should be positioned as a pure-software commodity-CXL runtime, not a custom hardware endpoint design.
- The three technical pillars are layered placement, SQ8/VNNI coarse-to-exact search, and morsel-driven issue/consume scheduling.
- The evaluation plan now requires FAISS baselines in addition to HNSWLib and DiskANN-vamana.
- Several numbers are paper-critical and must remain tied to canonical artifacts: 2.71x-6.37x QPS over HNSWLib, 3.09x-6.78x latency reduction over DiskANN-vamana, 0.38 pp recall gap, 59% scaling efficiency, and 27.1x ablation gap.

## Open Items

- Produce memory-tier landscape and Pareto figures.
- Run FAISS baseline experiments.
- Collect sensitivity and bottleneck profile results.
- Decide whether CXL-ANNS reproduction remains a P2 experiment or is discussed only as related work.

