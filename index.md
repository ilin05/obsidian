# Index

This file is the primary catalog for the vault's durable wiki content.

## Overview

- [Vector Quantization Research Overview](wiki/overview/vector-quantization-research-overview.md) - Working overview of the current quantization cluster spanning retrieval and KV-cache compression.

## Topics

- [Vector Quantization](wiki/topics/vector-quantization.md) - Core synthesis page for the current quantization cluster across retrieval and inference.
- [KV Cache Quantization](wiki/topics/kv-cache-quantization.md) - Notes on low-bit KV-cache compression with an emphasis on inner-product preservation.
- [Approximate Nearest Neighbor Search](wiki/topics/approximate-nearest-neighbor-search.md) - Unified synthesis of ANN quantization methods and graph/system serving designs.
- [Second-tier Memory for Vector Search](wiki/topics/second-tier-memory-for-vector-search.md) - System-level framing of SSD vs CXL/second-tier-memory trade-offs for billion-scale ANN.
- [Disaggregated Memory Vector Search](wiki/topics/disaggregated-memory-vector-search.md) - CXL/RDMA-oriented ANN serving patterns, including caching, layout, and pipeline design.
- [CXL Disaggregated Memory Systems](wiki/topics/cxl-disaggregated-memory-systems.md) - Cross-domain CXL system patterns spanning ANN, databases, and serverless runtimes.

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
- [GustANN](wiki/entities/gustann.md) - GPU-centric, CPU-assisted SSD graph ANNS system for high-throughput billion-scale search.
- [FusionANNS](wiki/entities/fusionanns.md) - SSD+GPU collaborative filtering and reranking system for billion-scale vector search.
- [RUMMY](wiki/entities/rummy.md) - GPU-accelerated IVF query-processing system for datasets beyond GPU memory using reordered pipelining.
- [SmartANNS](wiki/entities/smartanns.md) - SmartSSD/NDP billion-point ANNS system using host coordination, task scheduling, and shard pruning.
- [VBASE](wiki/entities/vbase.md) - PostgreSQL-based vector database engine that unifies vector similarity and relational queries through relaxed monotonicity.
- [Starling](wiki/entities/starling.md) - Disk-resident graph-index framework for vector database segments using navigation graphs, block shuffling, and block search.
- [BANG](wiki/entities/bang.md) - Single-GPU graph ANNS system using compressed vectors on GPU and host-resident graph access.
- [SPFresh](wiki/entities/spfresh.md) - Disk-based billion-scale vector search system for incremental in-place updates via LIRE.
- [Patience in Proximity](wiki/entities/patience-in-proximity.md) - Saturation-based early termination heuristic for HNSW traversal.
- [ANSMET](wiki/entities/ansmet.md) - Near-memory ANN acceleration with hybrid early termination.
- [CXLfork](wiki/entities/cxlfork.md) - CXL-based remote process cloning interface for serverless-style workloads.
- [PolarCXLMem](wiki/entities/polarcxlmem.md) - CXL-switch-based disaggregated-memory architecture for cloud-native databases.
- [Morsel-driven Parallelism](wiki/entities/morsel-driven-parallelism.md) - NUMA-aware dynamic scheduling concept from database query execution.

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
- [High-Throughput, Cost-Effective Billion-Scale Vector Search with a Single GPU](wiki/source-notes/gustann-2025.md) - Source note on GustANN, an SSD graph ANNS system using GPU traversal, CPU-assisted transfer, and pivot search.
- [Towards High-throughput and Low-latency Billion-scale Vector Search via CPU/GPU Collaborative Filtering and Re-ranking](wiki/source-notes/fusionanns-2025.md) - Source note on FusionANNS, an SSD+GPU system using multi-tier indexing, heuristic reranking, and I/O deduplication.
- [Fast Vector Query Processing for Large Datasets Beyond GPU Memory with Reordered Pipelining](wiki/source-notes/rummy-2024.md) - Source note on RUMMY, a GPU/host-memory IVF query-processing system with reordered pipelining.
- [Scalable Billion-point Approximate Nearest Neighbor Search Using SmartSSDs](wiki/source-notes/smartanns-2024.md) - Source note on SmartANNS, a SmartSSD-based host/NDP billion-point ANNS system.
- [VBASE: Unifying Online Vector Similarity Search and Relational Queries via Relaxed Monotonicity](wiki/source-notes/vbase-2023.md) - Source note on VBASE and relaxed-monotonicity vector-relational query processing.
- [Starling: An I/O-Efficient Disk-Resident Graph Index Framework for High-Dimensional Vector Similarity Search on Data Segment](wiki/source-notes/starling-2024.md) - Source note on Starling's segment-level disk graph layout and block search.
- [BANG: Billion-Scale Approximate Nearest Neighbour Search using a Single GPU](wiki/source-notes/bang-2024.md) - Source note on BANG's single-GPU graph ANNS design with compressed vectors and host graph access.
- [SPFresh: Incremental In-Place Update for Billion-Scale Vector Search](wiki/source-notes/spfresh-2023.md) - Source note on SPFresh and LIRE for fresh updates in disk-based vector search.

## Analyses

- [Billion-Scale Vector Search Literature Map](wiki/analyses/billion-scale-vector-search-literature-map.md) - Research map of covered and not-yet-ingested billion-scale vector search systems.
