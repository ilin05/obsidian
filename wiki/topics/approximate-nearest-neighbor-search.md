---
id: approximate-nearest-neighbor-search
type: topic
status: active
created: 2026-04-08
updated: 2026-05-08
tags:
  - ann
  - retrieval
  - vector-search
  - systems
source_count: 21
sources:
  - raw/sources/papers/product-quantization-2011.pdf
  - raw/sources/papers/rabitq-2024.pdf
  - raw/sources/papers/turboquant-2025.pdf
  - raw/sources/papers/hnsw-2016.pdf
  - raw/sources/papers/nsg-2019.pdf
  - raw/sources/papers/diskann-2019.pdf
  - raw/sources/papers/cxl-anns-2024.pdf
  - raw/sources/papers/performance-index-size-dilemma-2024.pdf
  - raw/sources/papers/d-hnsw-2025.pdf
  - raw/sources/papers/ansmet-2025.pdf
  - raw/sources/papers/patience-in-proximity-2025.pdf
  - raw/sources/papers/faiss-gpu-2017.pdf
  - raw/sources/papers/faiss-library-2025.pdf
  - raw/sources/papers/scann-2020.pdf
  - raw/sources/papers/spann-2021.pdf
  - raw/sources/papers/milvus-2021.pdf
  - raw/sources/papers/ann-benchmarks-2018.pdf
  - raw/sources/papers/cxl-memory-characterization-2023.pdf
  - vldb-paper-outline.md
  - vldb-paper-intro-draft.md
  - cross-query-rerank-coalescing-plan.md
related:
  - product-quantization
  - rabitq
  - turboquant
  - hnsw
  - nsg
  - diskann
  - faiss
  - scann
  - spann
  - milvus
  - ann-benchmarks
  - cxl-anns
  - d-hnsw
  - cxl-vector
  - cxl-vector-commodity-cxl-hnsw-serving
  - cross-query-rerank-coalescing
  - patience-in-proximity
  - ansmet
  - second-tier-memory-for-vector-search
  - disaggregated-memory-vector-search
  - vector-quantization
confidence: high
---

# Approximate Nearest Neighbor Search

## Summary

ANN search in this vault spans three layers: compression methods, graph/index methods, and systems/memory-tier co-design.

- **Compression-driven:** PQ, RaBitQ, TurboQuant, ScaNN, and FAISS SQ8/PQ.
- **Graph-index methods:** HNSW, NSG, Vamana/DiskANN.
- **Systems/memory-tier:** DiskANN and SPANN for SSD/DRAM+SSD, CXL-ANNS and d-HNSW for disaggregated memory, and CXL-Vector for commodity-CXL HNSW serving.
- **Production systems:** FAISS as a library baseline and Milvus as a vector DBMS reference.
- **Evaluation infrastructure:** ANN-Benchmarks for in-memory Pareto evaluation, with limitations for CXL/out-of-DRAM systems.

## Current View

**Library baseline:** [FAISS](../entities/faiss.md) is the canonical ANN library. In the current CXL-Vector paper plan, FAISS is no longer just background; `IndexHNSWSQ` and `IndexRefine` are planned baselines for the SQ8 coarse-to-exact design space.

**In-memory state of the art:** [ScaNN](../entities/scann.md), [HNSW](../entities/hnsw.md), and [NSG](../entities/nsg.md) define the high-recall in-memory frontier. [Patience in Proximity](../entities/patience-in-proximity.md) adds a lightweight early-termination branch directly relevant to HNSW traversal.

**SSD/second-tier tier:** [DiskANN](../entities/diskann.md) and [SPANN](../entities/spann.md) show how ANN systems adapt to storage tiers with high latency and coarse access granularity. SPANN remains the key DRAM+SSD comparison point.

**CXL/disaggregated tier:** [CXL-ANNS](../entities/cxl-anns.md) explores hardware/software co-design for CXL ANN. [d-HNSW](../entities/d-hnsw.md) targets RDMA-disaggregated HNSW. [CXL-Vector](../entities/cxl-vector.md) is now framed as a commodity-CXL, pure-software HNSW serving runtime whose central claim is that remaining CXL access must be scheduled, not merely placed.

**Near-memory:** [ANSMET](../entities/ansmet.md) uses near-memory processing and hybrid early termination, giving a useful contrast for where computation happens.

## Key System Progression

DiskANN/SPANN show how to survive SSD latency with coarse-grained access. CXL-ANNS shows what custom CXL endpoint compute can do. CXL-Vector asks a narrower but deployable question: how far pure software can push HNSW serving on commodity CXL memory.

## Current CXL-Vector Position

The latest CXL-Vector documents sharpen the contribution around:

- build-to-serve layout conversion;
- SQ8/VNNI coarse traversal;
- bounded exact rerank as the only CXL raw-vector stage;
- morsel-driven issue/consume scheduling;
- a possible cross-query rerank coalescing mechanism if overlap measurements justify it.

## Open Questions

- At what dataset scale and hardware budget does DRAM+CXL outperform DRAM+SSD?
- Can FAISS `IndexHNSWSQ` and `IndexRefine` establish a clean in-DRAM SQ8 baseline?
- Does cross-query rerank candidate overlap appear often enough to justify coalescing?
- How robust are current benchmark claims under mixed workloads with frequent inserts/updates?

## Related Pages

- [FAISS](../entities/faiss.md) · [ScaNN](../entities/scann.md) · [SPANN](../entities/spann.md) · [Milvus](../entities/milvus.md)
- [Product Quantization](../entities/product-quantization.md) · [RaBitQ](../entities/rabitq.md) · [TurboQuant](../entities/turboquant.md)
- [HNSW](../entities/hnsw.md) · [NSG](../entities/nsg.md) · [DiskANN](../entities/diskann.md)
- [CXL-ANNS](../entities/cxl-anns.md) · [d-HNSW](../entities/d-hnsw.md) · [ANSMET](../entities/ansmet.md)
- [CXL-Vector](../entities/cxl-vector.md)
- [Commodity CXL HNSW Serving](cxl-vector-commodity-cxl-hnsw-serving.md)
- [Cross-Query Rerank Coalescing](cross-query-rerank-coalescing.md)
- [Second-tier Memory for Vector Search](second-tier-memory-for-vector-search.md)
- [Disaggregated Memory Vector Search](disaggregated-memory-vector-search.md)

