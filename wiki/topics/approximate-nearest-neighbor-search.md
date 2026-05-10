---
id: approximate-nearest-neighbor-search
type: topic
status: active
created: 2026-04-08
updated: 2026-05-10
tags:
  - ann
  - retrieval
  - vector-search
  - systems
source_count: 44
sources:
  - raw/sources/papers/graph-based-anns-survey-2021.pdf
  - raw/sources/papers/nn-descent-2011.pdf
  - raw/sources/papers/gnns-knn-graph-2011.pdf
  - raw/sources/papers/efanna-2016.pdf
  - raw/sources/papers/fanng-2016.pdf
  - raw/sources/papers/dpg-ann-experiments-2016.pdf
  - raw/sources/papers/ngt-onng-2018.pdf
  - raw/sources/papers/flann-2014.pdf
  - raw/sources/papers/falconn-lsh-angular-2015.pdf
  - raw/sources/papers/kleinberg-small-world-2000.pdf
  - raw/sources/papers/navigable-small-world-graph-ann-2014.pdf
  - raw/sources/papers/monotonic-proximity-graphs-2021.pdf
  - raw/sources/papers/satellite-system-graph-2019.pdf
  - raw/sources/papers/relative-nn-descent-2023.pdf
  - raw/sources/papers/rnsg-2026.pdf
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
  - raw/sources/papers/svfusion-2026.pdf
  - raw/sources/papers/tribase-2025.pdf
  - raw/sources/papers/leann-2025.pdf
related:
  - product-quantization
  - rabitq
  - turboquant
  - hnsw
  - nsg
  - diskann
  - proximity-graph-theory-for-ann
  - monotonic-relative-neighborhood-graph
  - navigable-small-world-graph
  - satellite-system-graph
  - relative-nn-descent
  - rnsg
  - faiss
  - scann
  - spann
  - milvus
  - ann-benchmarks
  - ann-benchmarking-methodology
  - nn-descent
  - gnns
  - efanna
  - fanng
  - diversified-proximity-graph
  - ngt-onng
  - flann
  - falconn
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
  - svfusion
  - patience-in-proximity
  - ansmet
  - tribase
  - leann
  - second-tier-memory-for-vector-search
  - disaggregated-memory-vector-search
  - vector-quantization
  - vector-compression
confidence: high
---

# Approximate Nearest Neighbor Search

## Summary

ANN search in this vault spans three layers: compression methods, graph/index methods, and systems/memory-tier co-design.

- **Compression-driven:** PQ, RaBitQ, TurboQuant, ScaNN, and FAISS SQ8/PQ.
- **Graph-index methods:** HNSW, NSG, Vamana/DiskANN, now grounded by a proximity-graph-theory branch covering NN-Descent/KGraph, GNNS, NSW, EFANNA, FANNG, DPG, NGT/ONNG, MRNG, SSG, RNN-Descent, and RNSG.
- **Systems/memory-tier:** DiskANN, SPANN, Starling, and SPFresh for SSD/DRAM+SSD; RUMMY and BANG for GPU/host-memory execution; GustANN and FusionANNS for SSD+GPU collaboration; SmartANNS for SmartSSD/NDP; and CXL-ANNS/d-HNSW for disaggregated memory.
- **Production systems:** FAISS as a library baseline and Milvus as a vector DBMS reference.
- **Query semantics and freshness:** VBASE for vector-relational query processing and SPFresh for incremental updates.
- **Pruning and storage efficiency:** TriBase for lossless triangle-inequality pruning in cluster-based indexes and LEANN for storage-efficient graph indexes via embedding recomputation.
- **Evaluation infrastructure:** ANN-Benchmarks for in-memory Pareto evaluation, Graph-Based ANNS Survey 2021 for graph-component attribution, and SVFusion/SPFresh-style streaming metrics for update-heavy systems.
- **Non-graph baselines:** FLANN and FALCONN keep the benchmark layer connected to tree/auto-tuning and LSH/angular-distance methods.

## Current View

**Library baseline:** [FAISS](../entities/faiss.md) is the canonical ANN library, including HNSW, IVF/PQ/SQ variants, and refinement patterns that recur across many ANN systems.

**In-memory state of the art:** [ScaNN](../entities/scann.md), [HNSW](../entities/hnsw.md), and [NSG](../entities/nsg.md) define the high-recall in-memory frontier. [Patience in Proximity](../entities/patience-in-proximity.md) adds a lightweight early-termination branch directly relevant to HNSW traversal.

**Graph-theoretic foundations:** [Proximity Graph Theory for ANN](proximity-graph-theory-for-ann.md) collects the graph-theory layer behind graph ANN search: small-world navigability, approximate Delaunay intuition, monotonic search networks, MRNG, SSG, RNG Strategy, and range-aware RRNG/RNSG. This branch explains why graph quality is not just degree count or diameter; local search must be able to find useful paths.

**Graph ANN taxonomy:** [Graph-Based ANNS Survey 2021](../source-notes/graph-based-anns-survey-2021.md) provides a component-level comparison of graph ANN methods, useful for separating base graph choice, neighbor selection, connectivity repair, seed selection, and routing.

**Graph construction and pruning lineage:** [NN-Descent](../entities/nn-descent.md) supplies the KNNG construction primitive behind KGraph-style baselines and many refinement pipelines. [GNNS](../entities/gnns.md) captures the early random-start hill-climbing pattern. [EFANNA](../entities/efanna.md) shows how tree initialization improves construction and query seeds. [FANNG](../entities/fanng.md), [DPG](../entities/diversified-proximity-graph.md), and [NGT/ONNG](../entities/ngt-onng.md) connect practical pruning, degree control, and alternative-path removal to RNG/MRNG-style thinking.

**Benchmarking methodology:** [ANN Benchmarking Methodology](ann-benchmarking-methodology.md) now captures how to compare ANN systems across implementation benchmarking, graph-component attribution, and hardware/update-aware system evaluation. This prevents in-memory ANN-Benchmarks results from being overused as evidence for SSD, GPU, CXL, or streaming systems.

**Classical non-graph baselines:** [FLANN](../entities/flann.md) remains the canonical randomized KD-tree / k-means-tree auto-tuning library baseline. [FALCONN](../entities/falconn.md) is the practical cross-polytope LSH baseline for angular/cosine distance.

**SSD/second-tier tier:** [DiskANN](../entities/diskann.md), [SPANN](../entities/spann.md), [Starling](../entities/starling.md), and [SPFresh](../entities/spfresh.md) show how ANN systems adapt to storage tiers with high latency, coarse access granularity, segment constraints, and fresh-update pressure. SPANN remains the key DRAM+SSD comparison point.

**GPU/host-memory tier:** [RUMMY](../entities/rummy.md), [BANG](../entities/bang.md), and [SVFusion](../entities/svfusion.md) address datasets beyond GPU memory by coordinating host memory, GPU HBM, and PCIe transfer. RUMMY does this for IVF query processing; BANG does it for graph ANNS with compressed vectors on GPU; SVFusion adds streaming updates and CPU-GPU-disk consistency.

**SSD+GPU tier:** [GustANN](../entities/gustann.md) and [FusionANNS](../entities/fusionanns.md) show a newer heterogeneous branch. GustANN keeps graph traversal GPU-centric while using CPU-assisted selective SSD transfer; FusionANNS combines CPU/GPU filtering with selective raw-vector reranking and SSD I/O deduplication. Both papers turn the accurate/raw-vector stage into an explicitly scheduled data-movement problem.

**Near-data and database semantics:** [SmartANNS](../entities/smartanns.md) moves graph search into SmartSSDs and uses the host as a global coordinator. [VBASE](../entities/vbase.md) is not a storage-tier paper, but it clarifies how vector indexes should integrate with relational operators through relaxed monotonicity.

**CXL/disaggregated tier:** [CXL-ANNS](../entities/cxl-anns.md) explores hardware/software co-design for CXL ANN. [d-HNSW](../entities/d-hnsw.md) targets RDMA-disaggregated HNSW.

**Near-memory:** [ANSMET](../entities/ansmet.md) uses near-memory processing and hybrid early termination, giving a useful contrast for where computation happens.

**Lossless pruning:** [TriBase](../entities/tribase.md) refines cluster-based indexes with multi-level partitioning and dual (distance + angle) triangle-inequality pruning, achieving >99.4% pruning without accuracy loss. Demonstrates that distance computation — which accounts for 99.7% of in-memory ANNS time — can be dramatically reduced through geometric reasoning.

**Storage-efficient indexing:** [LEANN](../entities/leann.md) eliminates vector storage entirely by recomputing embeddings on-the-fly at query time and applies high-degree-preserving graph pruning. Represents a new design point: trade a small amount of search latency (acceptable in RAG where generation dominates) for massive storage savings (up to 50×).

## Key System Progression

DiskANN/SPANN show how to survive SSD latency with coarse-grained access. Starling refines SSD graph search at the data-segment layout level, while SPFresh extends the SSD/cluster branch to fresh updates. RUMMY/BANG show how to use a GPU when the full dataset or graph cannot fit in HBM. SVFusion adds a newer streaming branch where GPU/CPU/disk placement and update consistency are part of the ANN protocol. GustANN/FusionANNS show how GPU collaboration can improve SSD-backed search when PCIe movement is carefully controlled. SmartANNS shows the near-data-processing branch. CXL-ANNS shows what custom CXL endpoint compute can do, while d-HNSW shows how graph traversal changes under RDMA-style memory disaggregation.

## Open Questions

- At what dataset scale and hardware budget does DRAM+CXL outperform DRAM+SSD?
- Can FAISS `IndexHNSWSQ` and `IndexRefine` establish a clean in-DRAM SQ8 baseline?
- How robust are current benchmark claims under mixed workloads with frequent inserts/updates?
- Which baselines are mandatory for a paper claim: pure TopK only, filtered queries, fresh updates, or batch throughput?
- When should graph pruning be justified through MRNG/SSG/RNG theory rather than only empirical recall-latency curves?
- What benchmark should be the default for streaming ANN now that update freshness, recall drift, and tail latency matter?

## Related Pages

- [FAISS](../entities/faiss.md) · [ScaNN](../entities/scann.md) · [SPANN](../entities/spann.md) · [Milvus](../entities/milvus.md)
- [Product Quantization](../entities/product-quantization.md) · [RaBitQ](../entities/rabitq.md) · [TurboQuant](../entities/turboquant.md)
- [HNSW](../entities/hnsw.md) · [NSG](../entities/nsg.md) · [DiskANN](../entities/diskann.md)
- [NN-Descent](../entities/nn-descent.md) · [GNNS](../entities/gnns.md) · [EFANNA](../entities/efanna.md) · [FANNG](../entities/fanng.md) · [DPG](../entities/diversified-proximity-graph.md) · [NGT/ONNG](../entities/ngt-onng.md)
- [Proximity Graph Theory for ANN](proximity-graph-theory-for-ann.md) · [Navigable Small World Graph](../entities/navigable-small-world-graph.md) · [Monotonic Relative Neighborhood Graph](../entities/monotonic-relative-neighborhood-graph.md)
- [Satellite System Graph](../entities/satellite-system-graph.md) · [Relative NN-Descent](../entities/relative-nn-descent.md) · [RNSG](../entities/rnsg.md)
- [ANN Benchmarking Methodology](ann-benchmarking-methodology.md) · [FLANN](../entities/flann.md) · [FALCONN](../entities/falconn.md)
- [RUMMY](../entities/rummy.md) · [BANG](../entities/bang.md) · [SVFusion](../entities/svfusion.md) · [GustANN](../entities/gustann.md) · [FusionANNS](../entities/fusionanns.md)
- [SmartANNS](../entities/smartanns.md) · [Starling](../entities/starling.md) · [SPFresh](../entities/spfresh.md) · [VBASE](../entities/vbase.md)
- [CXL-ANNS](../entities/cxl-anns.md) · [d-HNSW](../entities/d-hnsw.md) · [ANSMET](../entities/ansmet.md)
- [TriBase](../entities/tribase.md) · [LEANN](../entities/leann.md)
- [Second-tier Memory for Vector Search](second-tier-memory-for-vector-search.md)
- [Disaggregated Memory Vector Search](disaggregated-memory-vector-search.md)
- [Vector Compression](vector-compression.md)
