# Index

This file is the primary catalog for the vault's durable wiki content.

## Overview

- [Vector Quantization Research Overview](wiki/overview/vector-quantization-research-overview.md) - Working overview of the current quantization cluster spanning retrieval and KV-cache compression.
- [CXL-Vector VLDB Paper](wiki/overview/cxl-vector-vldb-paper.md) - Current paper-level entry point for the commodity-CXL HNSW serving runtime.

## Topics

- [Vector Quantization](wiki/topics/vector-quantization.md) - Core synthesis page for the current quantization cluster across retrieval and inference.
- [KV Cache Quantization](wiki/topics/kv-cache-quantization.md) - Notes on low-bit KV-cache compression with an emphasis on inner-product preservation.
- [Approximate Nearest Neighbor Search](wiki/topics/approximate-nearest-neighbor-search.md) - Unified synthesis of ANN quantization methods and graph/system serving designs.
- [Second-tier Memory for Vector Search](wiki/topics/second-tier-memory-for-vector-search.md) - System-level framing of SSD vs CXL/second-tier-memory trade-offs for billion-scale ANN.
- [Disaggregated Memory Vector Search](wiki/topics/disaggregated-memory-vector-search.md) - CXL/RDMA-oriented ANN serving patterns, including caching, layout, and pipeline design.
- [CXL Disaggregated Memory Systems](wiki/topics/cxl-disaggregated-memory-systems.md) - Cross-domain CXL system patterns spanning ANN, databases, and serverless runtimes.
- [Commodity CXL HNSW Serving](wiki/topics/cxl-vector-commodity-cxl-hnsw-serving.md) - Refreshed CXL-Vector system design centered on commodity CXL, HNSW, SQ8/VNNI, bounded rerank, and morsel execution.
- [Cross-Query Rerank Coalescing](wiki/topics/cross-query-rerank-coalescing.md) - Proposed morsel-local deduplication mechanism for reducing duplicate CXL raw-vector fetches.

## Entities

- [FAISS](wiki/entities/faiss.md) - Canonical open-source ANN library (IVFPQ, HNSW, SQ8) from Meta FAIR; foundation for most production vector systems.
- [ScaNN](wiki/entities/scann.md) - Google's anisotropic quantization ANN library; best in-DRAM speed-recall Pareto on standard benchmarks.
- [SPANN](wiki/entities/spann.md) - Microsoft's memory-disk IVF system (NeurIPS 2021, deployed in Bing); primary DRAM+SSD comparison point.
- [Milvus](wiki/entities/milvus.md) - Purpose-built production vector DBMS (SIGMOD 2021, Zilliz); reference for full-system context.
- [ANN-Benchmarks](wiki/entities/ann-benchmarks.md) - Standard in-memory ANN benchmarking framework; Pareto frontier evaluation protocol.
- [Product Quantization](wiki/entities/product-quantization.md) - Classical retrieval-oriented vector quantization method built around subspace codebooks.
- [RaBitQ](wiki/entities/rabitq.md) - Data-oblivious ANN quantization family extended here to multi-bit high-recall retrieval.
- [QJL](wiki/entities/qjl.md) - Quantized JL method for KV-cache quantization with zero metadata overhead as a core claim.
- [TurboQuant](wiki/entities/turboquant.md) - Online two-stage quantization method connecting MSE-optimal compression with QJL-style inner-product estimation.
- [HNSW](wiki/entities/hnsw.md) - Foundational hierarchical graph index for high-recall ANN traversal.
- [NSG](wiki/entities/nsg.md) - Practical billion-scale graph index built as an MRNG approximation.
- [DiskANN](wiki/entities/diskann.md) - SSD-resident billion-scale ANN system centered on Vamana graph indexing.
- [CXL-ANNS](wiki/entities/cxl-anns.md) - CXL memory disaggregation and software-hardware co-design for billion-point ANN.
- [d-HNSW](wiki/entities/d-hnsw.md) - RDMA-disaggregated adaptation of HNSW serving for lower network overhead.
- [Patience in Proximity](wiki/entities/patience-in-proximity.md) - Saturation-based early termination heuristic for HNSW traversal.
- [ANSMET](wiki/entities/ansmet.md) - Near-memory ANN acceleration with hybrid early termination.
- [CXLfork](wiki/entities/cxlfork.md) - CXL-based remote process cloning interface for serverless-style workloads.
- [PolarCXLMem](wiki/entities/polarcxlmem.md) - CXL-switch-based disaggregated-memory architecture for cloud-native databases.
- [CXL-Vector](wiki/entities/cxl-vector.md) - In-vault commodity-CXL HNSW serving runtime and current VLDB paper project.
- [Morsel-driven Parallelism](wiki/entities/morsel-driven-parallelism.md) - NUMA-aware dynamic scheduling concept adapted here to batched ANN search.

## Personal

- No personal knowledge pages yet.

## Source Notes

- [Billion-scale Similarity Search with GPUs](wiki/source-notes/faiss-gpu-2017.md) - Johnson et al. 2017; GPU k-selection (WarpSelect) and IVFPQ; foundational FAISS GPU paper.
- [The Faiss Library](wiki/source-notes/faiss-library-2025.md) - Douze et al. 2025; comprehensive index taxonomy, SQ8 description, IndexRefine pattern.
- [Accelerating Large-Scale Inference with Anisotropic Vector Quantization](wiki/source-notes/scann-2020.md) - Guo et al. ICML 2020; ScaNN anisotropic quantization for MIPS; best in-DRAM ANN baseline.
- [SPANN: Highly-Efficient Billion-scale Approximate Nearest Neighbor Search](wiki/source-notes/spann-2021.md) - Chen et al. NeurIPS 2021; memory-disk IVF deployed in Bing.
- [Milvus: A Purpose-Built Vector Data Management System](wiki/source-notes/milvus-2021.md) - Wang et al. SIGMOD 2021; production vector DBMS with dynamic data, heterogeneous compute.
- [ANN-Benchmarks: A Benchmarking Tool for Approximate Nearest Neighbor Algorithms](wiki/source-notes/ann-benchmarks-2018.md) - Aumüller et al. 2018; standard speed-recall benchmarking framework.
- [Demystifying CXL Memory with Genuine CXL-Ready Systems and Devices](wiki/source-notes/cxl-memory-characterization-2023.md) - Sun et al. Micro 2023; true CXL latency/bandwidth characterization.
- [QJL: 1-Bit Quantized JL Transform for KV Cache Quantization with Zero Overhead](wiki/source-notes/qjl-2024.md) - Source note on QJL as a low-overhead KV-cache quantization method.
- [Product Quantization for Nearest Neighbor Search](wiki/source-notes/product-quantization-for-nearest-neighbor-search-2011.md) - Foundational source note on PQ, ADC, and IVFADC for ANN search.
- [Practical and Asymptotically Optimal Quantization of High-Dimensional Vectors in Euclidean Space for Approximate Nearest Neighbor Search](wiki/source-notes/rabitq-extension-2024.md) - Source note on the multi-bit RaBitQ extension for high-recall ANN search.
- [TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate](wiki/source-notes/turboquant-2025.md) - Source note on a general online quantization framework spanning KV caches and retrieval.
- [Efficient and Robust Approximate Nearest Neighbor Search Using Hierarchical Navigable Small World Graphs](wiki/source-notes/hnsw-2016.md) - Source note on the HNSW graph hierarchy and routing heuristic.
- [Fast Approximate Nearest Neighbor Search With the Navigating Spreading-out Graph](wiki/source-notes/nsg-2019.md) - Source note on MRNG/NSG design for billion-scale graph-based ANN.
- [DiskANN: Fast Accurate Billion-point Nearest Neighbor Search on a Single Node](wiki/source-notes/diskann-2019.md) - Source note on SSD-resident high-recall ANN with Vamana.
- [Bridging Software-Hardware for CXL Memory Disaggregation in Billion-Scale Nearest Neighbor Search](wiki/source-notes/cxl-anns-2024.md) - Source note on CXL-aware ANN co-design.
- [Characterizing the Dilemma of Performance and Index Size in Billion-Scale Vector Search and Breaking It with Second-Tier Memory](wiki/source-notes/performance-index-size-dilemma-2024.md) - Source note on throughput-amplification trade-offs and second-tier-memory redesign.
- [Efficient Vector Search on Disaggregated Memory with d-HNSW](wiki/source-notes/d-hnsw-2025.md) - Source note on RDMA-disaggregated HNSW serving.
- [Patience in Proximity: A Simple Early Termination Strategy for HNSW Graph Traversal in Approximate k-NN Search](wiki/source-notes/patience-in-proximity-2025.md) - Source note on saturation-based HNSW traversal termination.
- [ANSMET: Approximate Nearest Neighbor Search with Near-Memory Processing and Hybrid Early Termination](wiki/source-notes/ansmet-2025.md) - Source note on near-memory ANN acceleration with partial-dimension/bit early termination.
- [CXLfork: Fast Remote Fork over CXL Fabrics](wiki/source-notes/cxlfork-2025.md) - Source note on CXL-based remote fork and cluster-wide state sharing.
- [Unlocking the Potential of CXL for Disaggregated Memory in Cloud-Native Databases](wiki/source-notes/polarcxlmem-2025.md) - Source note on PolarCXLMem and PolarRecv for CXL-native cloud databases.
- [Morsel-Driven Parallelism: A NUMA-Aware Query Evaluation Framework for the Many-Core Age](wiki/source-notes/morsel-driven-parallelism-2014.md) - Source note on the original morsel-driven scheduling paper.
- [CXL-Vector VLDB Outline](wiki/source-notes/cxl-vector-vldb-outline-2026.md) - Source note on the current paper outline, structure, contributions, and evaluation plan.
- [CXL-Vector Introduction Draft](wiki/source-notes/cxl-vector-introduction-draft-2026.md) - Source note on the current introduction narrative and reviewer-facing risks.
- [Cross-Query Rerank Coalescing Plan](wiki/source-notes/cross-query-rerank-coalescing-plan-2026.md) - Source note on the proposed CXL-specific rerank deduplication mechanism.

## Analyses

- [CXL-Vector VLDB Positioning](wiki/analyses/cxl-vector-vldb-positioning.md) - Durable synthesis of current paper claim, wording boundaries, reviewer questions, and evidence risks.
