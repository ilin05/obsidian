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
source_count: 26
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
  - raw/sources/papers/gustann-2025.pdf
  - raw/sources/papers/fusionanns-2025.pdf
  - raw/sources/papers/rummy-2024.pdf
  - raw/sources/papers/smartanns-2024.pdf
  - raw/sources/papers/vbase-2023.pdf
  - raw/sources/papers/starling-2024.pdf
  - raw/sources/papers/bang-2024.pdf
  - raw/sources/papers/spfresh-2023.pdf
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
  - gustann
  - fusionanns
  - rummy
  - smartanns
  - vbase
  - starling
  - bang
  - spfresh
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
- **Systems/memory-tier:** DiskANN, SPANN, Starling, and SPFresh for SSD/DRAM+SSD; RUMMY and BANG for GPU/host-memory execution; GustANN and FusionANNS for SSD+GPU collaboration; SmartANNS for SmartSSD/NDP; and CXL-ANNS/d-HNSW for disaggregated memory.
- **Production systems:** FAISS as a library baseline and Milvus as a vector DBMS reference.
- **Query semantics and freshness:** VBASE for vector-relational query processing and SPFresh for incremental updates.
- **Evaluation infrastructure:** ANN-Benchmarks for in-memory Pareto evaluation, with limitations for CXL/out-of-DRAM systems.

## Current View

**Library baseline:** [FAISS](../entities/faiss.md) is the canonical ANN library, including HNSW, IVF/PQ/SQ variants, and refinement patterns that recur across many ANN systems.

**In-memory state of the art:** [ScaNN](../entities/scann.md), [HNSW](../entities/hnsw.md), and [NSG](../entities/nsg.md) define the high-recall in-memory frontier. [Patience in Proximity](../entities/patience-in-proximity.md) adds a lightweight early-termination branch directly relevant to HNSW traversal.

**SSD/second-tier tier:** [DiskANN](../entities/diskann.md), [SPANN](../entities/spann.md), [Starling](../entities/starling.md), and [SPFresh](../entities/spfresh.md) show how ANN systems adapt to storage tiers with high latency, coarse access granularity, segment constraints, and fresh-update pressure. SPANN remains the key DRAM+SSD comparison point.

**GPU/host-memory tier:** [RUMMY](../entities/rummy.md) and [BANG](../entities/bang.md) address datasets beyond GPU memory by coordinating host memory, GPU HBM, and PCIe transfer. RUMMY does this for IVF query processing; BANG does it for graph ANNS with compressed vectors on GPU.

**SSD+GPU tier:** [GustANN](../entities/gustann.md) and [FusionANNS](../entities/fusionanns.md) show a newer heterogeneous branch. GustANN keeps graph traversal GPU-centric while using CPU-assisted selective SSD transfer; FusionANNS combines CPU/GPU filtering with selective raw-vector reranking and SSD I/O deduplication. Both papers turn the accurate/raw-vector stage into an explicitly scheduled data-movement problem.

**Near-data and database semantics:** [SmartANNS](../entities/smartanns.md) moves graph search into SmartSSDs and uses the host as a global coordinator. [VBASE](../entities/vbase.md) is not a storage-tier paper, but it clarifies how vector indexes should integrate with relational operators through relaxed monotonicity.

**CXL/disaggregated tier:** [CXL-ANNS](../entities/cxl-anns.md) explores hardware/software co-design for CXL ANN. [d-HNSW](../entities/d-hnsw.md) targets RDMA-disaggregated HNSW.

**Near-memory:** [ANSMET](../entities/ansmet.md) uses near-memory processing and hybrid early termination, giving a useful contrast for where computation happens.

## Key System Progression

DiskANN/SPANN show how to survive SSD latency with coarse-grained access. Starling refines SSD graph search at the data-segment layout level, while SPFresh extends the SSD/cluster branch to fresh updates. RUMMY/BANG show how to use a GPU when the full dataset or graph cannot fit in HBM. GustANN/FusionANNS show how GPU collaboration can improve SSD-backed search when PCIe movement is carefully controlled. SmartANNS shows the near-data-processing branch. CXL-ANNS shows what custom CXL endpoint compute can do, while d-HNSW shows how graph traversal changes under RDMA-style memory disaggregation.

## Open Questions

- At what dataset scale and hardware budget does DRAM+CXL outperform DRAM+SSD?
- Can FAISS `IndexHNSWSQ` and `IndexRefine` establish a clean in-DRAM SQ8 baseline?
- How robust are current benchmark claims under mixed workloads with frequent inserts/updates?
- Which baselines are mandatory for a paper claim: pure TopK only, filtered queries, fresh updates, or batch throughput?

## Related Pages

- [FAISS](../entities/faiss.md) · [ScaNN](../entities/scann.md) · [SPANN](../entities/spann.md) · [Milvus](../entities/milvus.md)
- [Product Quantization](../entities/product-quantization.md) · [RaBitQ](../entities/rabitq.md) · [TurboQuant](../entities/turboquant.md)
- [HNSW](../entities/hnsw.md) · [NSG](../entities/nsg.md) · [DiskANN](../entities/diskann.md)
- [RUMMY](../entities/rummy.md) · [BANG](../entities/bang.md) · [GustANN](../entities/gustann.md) · [FusionANNS](../entities/fusionanns.md)
- [SmartANNS](../entities/smartanns.md) · [Starling](../entities/starling.md) · [SPFresh](../entities/spfresh.md) · [VBASE](../entities/vbase.md)
- [CXL-ANNS](../entities/cxl-anns.md) · [d-HNSW](../entities/d-hnsw.md) · [ANSMET](../entities/ansmet.md)
- [Second-tier Memory for Vector Search](second-tier-memory-for-vector-search.md)
- [Disaggregated Memory Vector Search](disaggregated-memory-vector-search.md)
