# Index

This file is the primary catalog for the vault's durable wiki content.

## Operating Guides

- [ANNS System Agent Cache](CACHE.md) - Short entry point for agents working on ANNS system papers.
- [ANNS System Reviewer Guide](reviewer.md) - Reviewer protocol for applying the VLDB section-review skill with this ANNS knowledge base.
- [ANNS System Writer Guide](writer.md) - Writer protocol for turning reviews into conservative ANNS paper edits.

## Overview

- [Vector Quantization Research Overview](wiki/overview/vector-quantization-research-overview.md) - Working overview of the current quantization cluster spanning retrieval and KV-cache compression.

## Topics

- [Vector Quantization](wiki/topics/vector-quantization.md) - Core synthesis page for the current quantization cluster across retrieval and inference.
- [KV Cache Quantization](wiki/topics/kv-cache-quantization.md) - Notes on low-bit KV-cache compression with an emphasis on inner-product preservation.
- [Approximate Nearest Neighbor Search](wiki/topics/approximate-nearest-neighbor-search.md) - Unified synthesis of ANN quantization methods and graph/system serving designs.
- [ANN Benchmarking Methodology](wiki/topics/ann-benchmarking-methodology.md) - Evaluation framework for ANN implementation, graph-component, hardware-tier, and streaming-update claims.
- [Proximity Graph Theory for ANN](wiki/topics/proximity-graph-theory-for-ann.md) - Graph-theoretic foundations for ANN graph indexes, including small-world navigability, MRNG, SSG, RNG Strategy, and RNSG.
- [Second-tier Memory for Vector Search](wiki/topics/second-tier-memory-for-vector-search.md) - System-level framing of SSD vs CXL/second-tier-memory trade-offs for billion-scale ANN.
- [Disaggregated Memory Vector Search](wiki/topics/disaggregated-memory-vector-search.md) - CXL/RDMA-oriented ANN serving patterns, including caching, layout, and pipeline design.
- [CXL Disaggregated Memory Systems](wiki/topics/cxl-disaggregated-memory-systems.md) - Cross-domain CXL system patterns spanning ANN, databases, and serverless runtimes.

## Entities

- [FAISS](wiki/entities/faiss.md) - Canonical open-source ANN library (IVFPQ, HNSW, SQ8) from Meta FAIR; foundation for most production vector systems.
- [ScaNN](wiki/entities/scann.md) - Google's anisotropic quantization ANN library; best in-DRAM speed-recall Pareto on standard benchmarks.
- [SPANN](wiki/entities/spann.md) - Microsoft's memory-disk IVF system (NeurIPS 2021, deployed in Bing); primary DRAM+SSD comparison point.
- [Milvus](wiki/entities/milvus.md) - Purpose-built production vector DBMS (SIGMOD 2021, Zilliz); reference for full-system context.
- [ANN-Benchmarks](wiki/entities/ann-benchmarks.md) - Standard in-memory ANN benchmarking framework; Pareto frontier evaluation protocol.
- [FLANN](wiki/entities/flann.md) - Classical auto-tuning ANN library built around randomized KD forests and priority search k-means trees.
- [FALCONN](wiki/entities/falconn.md) - Practical cross-polytope LSH library for angular/cosine-distance nearest neighbor search.
- [Product Quantization](wiki/entities/product-quantization.md) - Classical retrieval-oriented vector quantization method built around subspace codebooks.
- [RaBitQ](wiki/entities/rabitq.md) - Data-oblivious ANN quantization family extended here to multi-bit high-recall retrieval.
- [QJL](wiki/entities/qjl.md) - Quantized JL method for KV-cache quantization with zero metadata overhead as a core claim.
- [TurboQuant](wiki/entities/turboquant.md) - Online two-stage quantization method connecting MSE-optimal compression with QJL-style inner-product estimation.
- [HNSW](wiki/entities/hnsw.md) - Foundational hierarchical graph index for high-recall ANN traversal.
- [NSG](wiki/entities/nsg.md) - Practical billion-scale graph index built as an MRNG approximation.
- [NN-Descent](wiki/entities/nn-descent.md) - Approximate KNN graph construction primitive behind KGraph-style baselines and graph-pruning pipelines.
- [GNNS](wiki/entities/gnns.md) - Early KNN-graph ANN search method using random-start greedy hill climbing.
- [EFANNA](wiki/entities/efanna.md) - Tree-initialized KNN graph construction and search method bridging FLANN-like trees and graph ANN.
- [FANNG](wiki/entities/fanng.md) - RNG-like graph ANN pruning method based on occlusion-style redundant-edge removal.
- [Diversified Proximity Graph](wiki/entities/diversified-proximity-graph.md) - DPG graph ANN method connecting broad empirical benchmarking with diversified graph pruning.
- [NGT / ONNG](wiki/entities/ngt-onng.md) - Practical graph-library lineage focused on degree adjustment, path adjustment, and graph traversal optimization.
- [Monotonic Relative Neighborhood Graph](wiki/entities/monotonic-relative-neighborhood-graph.md) - Theoretical proximity-graph target behind NSG-style monotonic graph ANN search.
- [Navigable Small World Graph](wiki/entities/navigable-small-world-graph.md) - Early small-world graph ANN method and direct predecessor to HNSW.
- [Satellite System Graph](wiki/entities/satellite-system-graph.md) - NSG follow-up that uses directionally balanced outgoing edges and MSNET-style analysis.
- [Relative NN-Descent](wiki/entities/relative-nn-descent.md) - Graph-index construction method combining NN-Descent with RNG Strategy.
- [RNSG](wiki/entities/rnsg.md) - Range-aware graph index for range-filtered ANN built around RRNG/RNSG theory.
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
- [SVFusion](wiki/entities/svfusion.md) - CPU-GPU-disk streaming vector search system with workload-aware GPU placement and update consistency.
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
- [Scalable Nearest Neighbor Algorithms for High Dimensional Data](wiki/source-notes/flann-2014.md) - Muja and Lowe TPAMI 2014; FLANN randomized KD forest, priority search k-means tree, and automatic configuration.
- [Practical and Optimal LSH for Angular Distance](wiki/source-notes/falconn-lsh-angular-2015.md) - Andoni et al. 2015; FALCONN/cross-polytope LSH for angular-distance ANN.
- [Demystifying CXL Memory with Genuine CXL-Ready Systems and Devices](wiki/source-notes/cxl-memory-characterization-2023.md) - Sun et al. Micro 2023; true CXL latency/bandwidth characterization.
- [QJL: 1-Bit Quantized JL Transform for KV Cache Quantization with Zero Overhead](wiki/source-notes/qjl-2024.md) - Source note on QJL as a low-overhead KV-cache quantization method.
- [Product Quantization for Nearest Neighbor Search](wiki/source-notes/product-quantization-for-nearest-neighbor-search-2011.md) - Foundational source note on PQ, ADC, and IVFADC for ANN search.
- [Practical and Asymptotically Optimal Quantization of High-Dimensional Vectors in Euclidean Space for Approximate Nearest Neighbor Search](wiki/source-notes/rabitq-extension-2024.md) - Source note on the multi-bit RaBitQ extension for high-recall ANN search.
- [TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate](wiki/source-notes/turboquant-2025.md) - Source note on a general online quantization framework spanning KV caches and retrieval.
- [Efficient and Robust Approximate Nearest Neighbor Search Using Hierarchical Navigable Small World Graphs](wiki/source-notes/hnsw-2016.md) - Source note on the HNSW graph hierarchy and routing heuristic.
- [Fast Approximate Nearest Neighbor Search With the Navigating Spreading-out Graph](wiki/source-notes/nsg-2019.md) - Source note on MRNG/NSG design for billion-scale graph-based ANN.
- [A Comprehensive Survey and Experimental Comparison of Graph-Based Approximate Nearest Neighbor Search](wiki/source-notes/graph-based-anns-survey-2021.md) - PVLDB 2021 survey note on graph ANN taxonomy, base graphs, and component-level evaluation.
- [Efficient K-Nearest Neighbor Graph Construction for Generic Similarity Measures](wiki/source-notes/nn-descent-2011.md) - Dong, Charikar, and Li WWW 2011; NN-Descent approximate KNN graph construction.
- [Fast Approximate Nearest-Neighbor Search with k-Nearest Neighbor Graph](wiki/source-notes/gnns-knn-graph-2011.md) - Hajebi et al. IJCAI 2011; early GNNS graph traversal over KNN graphs.
- [EFANNA: An Extremely Fast Approximate Nearest Neighbor Search Algorithm Based on kNN Graph](wiki/source-notes/efanna-2016.md) - Fu and Cai 2016; tree-initialized KNN graph construction and graph search.
- [FANNG: Fast Approximate Nearest Neighbour Graphs](wiki/source-notes/fanng-2016.md) - Harwood and Drummond CVPR 2016; occlusion/RNG-like graph pruning for ANN.
- [Approximate Nearest Neighbor Search on High Dimensional Data - Experiments, Analyses, and Improvement](wiki/source-notes/dpg-ann-experiments-2016.md) - Li et al. 2016; broad ANNS empirical study and DPG graph improvement.
- [Optimization of Indexing Based on k-Nearest Neighbor Graph for Proximity Search in High-dimensional Data](wiki/source-notes/ngt-onng-2018.md) - Iwasaki and Miyazaki 2018; NGT/ONNG degree and path adjustment for graph search.
- [The Small-World Phenomenon: An Algorithmic Perspective](wiki/source-notes/kleinberg-small-world-2000.md) - Kleinberg 2000 source note on decentralized navigability and long-range links.
- [Approximate Nearest Neighbor Algorithm Based on Navigable Small World Graphs](wiki/source-notes/navigable-small-world-graph-ann-2014.md) - Malkov et al. 2014 source note on NSW as the direct HNSW predecessor.
- [Understanding and Generalizing Monotonic Proximity Graphs](wiki/source-notes/monotonic-proximity-graphs-2021.md) - Source note on MRNG theory, degree-bounded generalization, and conflicting nodes.
- [High Dimensional Similarity Search with Satellite System Graph](wiki/source-notes/satellite-system-graph-2019.md) - Source note on SSG/NSSG as an MSNET-style follow-up to NSG.
- [Relative NN-Descent](wiki/source-notes/relative-nn-descent-2023.md) - Source note on faster graph-index construction via NN-Descent plus RNG Strategy.
- [RNSG: A Range-Aware Graph Index for Efficient Range-Filtered Approximate Nearest Neighbor Search](wiki/source-notes/rnsg-2026.md) - Source note on RRNG/RNSG for range-filtered ANN.
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
- [SVFusion: A CPU-GPU Co-Processing Architecture for Large-Scale Real-Time Vector Search](wiki/source-notes/svfusion-2026.md) - Source note on SVFusion's CPU-GPU-disk streaming ANN design.

## Analyses

- [Billion-Scale Vector Search Literature Map](wiki/analyses/billion-scale-vector-search-literature-map.md) - Research map of covered and not-yet-ingested billion-scale vector search systems.
- [Graph-Theoretic ANN Inbox Candidates](wiki/analyses/graph-theoretic-ann-inbox-candidates.md) - Candidate queue and rationale for proximity/RNG/MRNG/small-world graph papers, now including the first ingested batch.
- [ANN Benchmarking And Graph Coverage Gaps](wiki/analyses/ann-benchmarking-and-graph-coverage-gaps.md) - Gap analysis and inbox candidate queue seeded by ANN-Benchmarks and the graph-based ANNS survey.
