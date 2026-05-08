---
id: cxl-vector-vldb-paper
type: overview
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - cxl-vector
  - vldb
  - ann
  - paper
source_count: 3
sources:
  - vldb-paper-outline.md
  - vldb-paper-intro-draft.md
  - cross-query-rerank-coalescing-plan.md
related:
  - cxl-vector
  - cxl-vector-commodity-cxl-hnsw-serving
  - cross-query-rerank-coalescing
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# CXL-Vector VLDB Paper

## Current Thesis

CXL-Vector is now framed as a VLDB paper about a pure-software HNSW serving runtime for commodity CXL memory. The core claim is that putting HNSW on CXL is not only a placement problem; it becomes a scheduling problem once PCIe bandwidth and remote raw-vector fetches dominate batched serving.

## Paper Arc

1. **Promise:** CXL offers a byte-addressable memory tier between DRAM and SSD.
2. **Collapse:** naive CXL-HNSW saturates PCIe bandwidth and stops scaling.
3. **Insight:** raw-vector access must be confined to bounded rerank and then scheduled across queries.
4. **System:** CXL-Vector combines layered placement, SQ8/VNNI coarse search, bounded exact rerank, and morsel-driven issue/consume execution.
5. **Evaluation:** the planned paper emphasizes eight ANN datasets, HNSWLib/DiskANN/FAISS baselines, thread scaling, ablations, physical NUMA verification, and memory-footprint accounting.

## Contribution Boundary

- This is not a general vector database.
- This is not a new ANN graph construction algorithm.
- This is not a hardware-offload design like CXL-ANNS.
- The strongest position is a commodity-CXL, pure-software serving runtime that makes remote-memory bandwidth an explicit execution constraint.

## New Since The Previous Wiki State

- The paper narrative is now venue-shaped around PVLDB/VLDB.
- The system motivation is sharpened around three failure modes: unconstrained remote access, single-query blocking, and unmanaged thread-level bandwidth pressure.
- FAISS baselines are now explicitly part of the planned evaluation.
- Cross-query rerank coalescing is a new candidate mechanism to strengthen the CXL-specific novelty story.

## Open Work

- Run and lock FAISS `IndexHNSWSQ` and `IndexRefine` baselines.
- Produce sensitivity curves for morsel size, rerank budget, `ef_search`, and patience.
- Measure rerank-candidate overlap before implementing cross-query coalescing.
- Keep claims that cite 27.1x, 59% scaling efficiency, and 0.38 pp recall gap tied to canonical result artifacts.

## Related Pages

- [CXL-Vector](../entities/cxl-vector.md)
- [Commodity CXL HNSW Serving](../topics/cxl-vector-commodity-cxl-hnsw-serving.md)
- [Cross-Query Rerank Coalescing](../topics/cross-query-rerank-coalescing.md)
- [Paper Positioning](../analyses/cxl-vector-vldb-positioning.md)

