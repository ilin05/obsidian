# ANNS System Agent Cache

Shortest read path for agents working on any ANNS system paper.

## Start Here

1. [index.md](index.md) - full vault catalog.
2. [Approximate Nearest Neighbor Search](wiki/topics/approximate-nearest-neighbor-search.md) - main ANNS synthesis.
3. [ANN Benchmarking Methodology](wiki/topics/ann-benchmarking-methodology.md) - required evaluation rules.
4. [Graph-Based ANNS Survey 2021](wiki/source-notes/graph-based-anns-survey-2021.md) - graph taxonomy and components.
5. [Proximity Graph Theory for ANN](wiki/topics/proximity-graph-theory-for-ann.md) - graph searchability, RNG/MRNG/SSG/RNSG.
6. [ANNS Figure Drawing Cache](FIGURE_CACHE.md) - quick entry for drawing and reviewing paper figures.

## Pick By Paper Claim

- In-memory graph ANN: [HNSW](wiki/entities/hnsw.md), [NSG](wiki/entities/nsg.md), [NN-Descent](wiki/entities/nn-descent.md), [FANNG](wiki/entities/fanng.md), [NGT/ONNG](wiki/entities/ngt-onng.md).
- Quantization/library baseline: [FAISS](wiki/entities/faiss.md), [ScaNN](wiki/entities/scann.md), [Product Quantization](wiki/entities/product-quantization.md), [RaBitQ](wiki/entities/rabitq.md).
- Non-graph baselines: [FLANN](wiki/entities/flann.md), [FALCONN](wiki/entities/falconn.md).
- SSD/second-tier: [Second-tier Memory](wiki/topics/second-tier-memory-for-vector-search.md), [DiskANN](wiki/entities/diskann.md), [SPANN](wiki/entities/spann.md), [Starling](wiki/entities/starling.md), [SPFresh](wiki/entities/spfresh.md).
- GPU/heterogeneous: [BANG](wiki/entities/bang.md), [RUMMY](wiki/entities/rummy.md), [GustANN](wiki/entities/gustann.md), [FusionANNS](wiki/entities/fusionanns.md), [SVFusion](wiki/entities/svfusion.md).
- CXL/RDMA/NDP: [Disaggregated Memory Vector Search](wiki/topics/disaggregated-memory-vector-search.md), [CXL-ANNS](wiki/entities/cxl-anns.md), [d-HNSW](wiki/entities/d-hnsw.md), [SmartANNS](wiki/entities/smartanns.md).
- Vector DB semantics: [Milvus](wiki/entities/milvus.md), [VBASE](wiki/entities/vbase.md), [RNSG](wiki/entities/rnsg.md).
- Vector compression: [Vector Compression](wiki/topics/vector-compression.md), [DeltaPQ](wiki/entities/deltapq.md), [SegPQ](wiki/entities/segpq.md), [LEP](wiki/entities/lep.md), [LEANN](wiki/entities/leann.md).
- Lossless float compression: [Lossless Floating Point Compression](wiki/topics/lossless-floating-point-compression.md), [Gorilla](wiki/source-notes/gorilla-2015.md), [Chimp](wiki/source-notes/chimp-2022.md), [Elf](wiki/source-notes/elf-2023.md), [ALP](wiki/source-notes/alp-2023.md), [Camel](wiki/source-notes/camel-2024.md), [DeXOR](wiki/source-notes/dexor-2025.md).

## Claim Boundaries

- Compare at matched recall; raw QPS without quality alignment is weak.
- Separate algorithm, implementation, and hardware effects.
- Report build time, memory/index size, update cost, and parameter settings when relevant.
- For systems claims, report data movement: SSD I/O, PCIe traffic, CXL/RDMA traffic, GPU HBM use, or cache hit rate.
- Do not invent numbers, baselines, experiments, or citations.

## Writing/Review Protocol

- Review guide: [reviewer.md](reviewer.md)
- Writer guide: [writer.md](writer.md)
- Figure guide: [FIGURE_CACHE.md](FIGURE_CACHE.md)
