---
id: cxl-vector
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - cxl
  - hnsw
  - systems
source_count: 3
sources:
  - vldb-paper-outline.md
  - vldb-paper-intro-draft.md
  - cross-query-rerank-coalescing-plan.md
related:
  - cxl-vector-vldb-paper
  - cxl-vector-commodity-cxl-hnsw-serving
  - cross-query-rerank-coalescing
  - hnsw
  - faiss
  - spann
  - cxl-anns
confidence: high
---

# CXL-Vector

## Profile

CXL-Vector is the in-vault research project for a pure-software HNSW serving runtime on commodity CXL memory. The current paper framing targets PVLDB/VLDB and positions the system as a response to PCIe bandwidth saturation under batched ANN serving.

## Current System Claim

CXL-Vector separates hot routing state from cold exact vectors:

- DRAM holds SQ8 codes, SQ8 metadata, graph edges, upper links, and other routing metadata.
- CXL holds raw float vectors.
- Query execution uses SQ8/VNNI coarse traversal first, then bounded exact rerank as the only CXL-touching stage.
- Morsel-driven batch execution interleaves queries so CXL wait time can be overlapped with useful coarse-distance work.

## Paper-Facing Contributions

- A characterization of why naive CXL-HNSW fails under multi-threaded serving.
- A build-to-serve layout conversion that separates DRAM routing structures from CXL raw vectors.
- A coarse-to-exact search path with SQ8/VNNI traversal, patience termination, and bounded exact rerank.
- A `QueryState` plus issue/consume execution model driven by morsel scheduling.
- Physical NUMA verification that raw vectors are actually placed on the CXL node.

## Current Evidence Claims

- Main-result target: 2.71x-6.37x QPS over HNSWLib.
- DiskANN-vamana latency target: 3.09x-6.78x reduction.
- Recall target: within 0.38 percentage points of the best baseline.
- Thread-scaling target: near-linear scaling to 56 threads with 59% efficiency.
- Ablation target: 27.1x gap between full path and scheduler/SQ8-off path at 56 threads.

These numbers should remain attached to canonical experiment artifacts before paper freeze.

## Active Research Risk

The strongest unresolved novelty question is whether the scheduler can support a CXL-specific mechanism beyond overlap. [Cross-query rerank coalescing](../topics/cross-query-rerank-coalescing.md) is the current candidate: it uses morsel-local batch structure to deduplicate CXL raw-vector fetches across queries.

## Related Pages

- [CXL-Vector VLDB Paper](../overview/cxl-vector-vldb-paper.md)
- [Commodity CXL HNSW Serving](../topics/cxl-vector-commodity-cxl-hnsw-serving.md)
- [Cross-Query Rerank Coalescing](../topics/cross-query-rerank-coalescing.md)
- [Paper Positioning](../analyses/cxl-vector-vldb-positioning.md)

