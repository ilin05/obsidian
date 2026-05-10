# Index

This file is the primary catalog for the vault's durable wiki content.

## Operating Guides

- [ANNS System Agent Cache](CACHE.md) - Short entry point for agents working on ANNS system papers.
- [ANNS System Reviewer Guide](reviewer.md) - Reviewer protocol for applying the VLDB section-review skill with this ANNS knowledge base.
- [ANNS System Writer Guide](writer.md) - Writer protocol for turning reviews into conservative ANNS paper edits.
- [ANNS Figure Drawing Cache](FIGURE_CACHE.md) - Fast entry point for drawing and reviewing ANNS research figures.
- [ANNS Figure Drawing Skill](skills/anns-research-figure-drawing/SKILL.md) - Vault-local skill for ANNS system, algorithm, and experiment figures.

## Overview

- [Vector Quantization Research Overview](wiki/overview/vector-quantization-research-overview.md) - Working overview of the current quantization cluster spanning retrieval and KV-cache compression.

## Topics

- [Vector Quantization](wiki/topics/vector-quantization.md) - Core synthesis page for the current quantization cluster across retrieval and inference.
- [KV Cache Quantization](wiki/topics/kv-cache-quantization.md) - Notes on low-bit KV-cache compression with an emphasis on inner-product preservation.
- [Approximate Nearest Neighbor Search](wiki/topics/approximate-nearest-neighbor-search.md) - Unified synthesis of ANN quantization methods and graph/system serving designs.
- [ANN Benchmarking Methodology](wiki/topics/ann-benchmarking-methodology.md) - Evaluation framework for ANN implementation, graph-component, hardware-tier, and streaming-update claims.
- [Proximity Graph Theory for ANN](wiki/topics/proximity-graph-theory-for-ann.md) - Graph-theoretic foundations for ANN graph indexes, including small-world navigability, MRNG, SSG, RNG Strategy, and RNSG.
- [SIMD and Vectorization for ANN Systems](wiki/topics/simd-and-vectorization-for-ann-systems.md) - CPU SIMD/vectorization execution layer for ANN kernels, compressed scans, filters, and cache-aware batching.
- [Scalar and Binary Quantization for ANN](wiki/topics/scalar-and-binary-quantization-for-ann.md) - Scalar, binary, and low-precision quantization branch for ANN memory reduction and distance-kernel acceleration.
- [Second-tier Memory for Vector Search](wiki/topics/second-tier-memory-for-vector-search.md) - System-level framing of SSD vs CXL/second-tier-memory trade-offs for billion-scale ANN.
- [Disaggregated Memory Vector Search](wiki/topics/disaggregated-memory-vector-search.md) - CXL/RDMA-oriented ANN serving patterns, including caching, layout, and pipeline design.
- [Lossless Floating Point Compression](wiki/topics/lossless-floating-point-compression.md) - Synthesis of XOR-based streaming float compression: Gorilla, Chimp, Elf, ALP, Camel, and DeXOR.
- [CXL Disaggregated Memory Systems](wiki/topics/cxl-disaggregated-memory-systems.md) - Cross-domain CXL system patterns spanning ANN, databases, and serverless runtimes.
- [Vector Compression](wiki/topics/vector-compression.md) - Synthesis of vector compression techniques for ANN search spanning PQ codes, codebooks, vector IDs, graph edges, and embedding recomputation.

## Entities

- [FAISS](wiki/entities/faiss.md) - Canonical open-source ANN library (IVFPQ, HNSW, SQ8) from Meta FAIR; foundation for most production vector systems.
- [ScaNN](wiki/entities/scann.md) - Google's anisotropic quantization ANN library; best in-DRAM speed-recall Pareto on standard benchmarks.
- [SPANN](wiki/entities/spann.md) - Microsoft's memory-disk IVF system (NeurIPS 2021, deployed in Bing); primary DRAM+SSD comparison point.
- [Milvus](wiki/entities/milvus.md) - Purpose-built production vector DBMS (SIGMOD 2021, Zilliz); reference for full-system context.
- [ANN-Benchmarks](wiki/entities/ann-benchmarks.md) - Standard in-memory ANN benchmarking framework; Pareto frontier evaluation protocol.
- [FLANN](wiki/entities/flann.md) - Classical auto-tuning ANN library built around randomized KD forests and priority search k-means trees.
- [FALCONN](wiki/entities/falconn.md) - Practical cross-polytope LSH library for angular/cosine-distance nearest neighbor search.
- [Product Quantization](wiki/entities/product-quantization.md) - Classical retrieval-oriented vector quantization method built around subspace codebooks.
- [PQ Fast Scan](wiki/entities/pq-fast-scan.md) - SIMD in-register lookup method for accelerating ADC over product-quantized ANN codes.
- [Quicker ADC](wiki/entities/quicker-adc.md) - AVX2/AVX-512-oriented generalization of Quick ADC for FAISS/PQ search.
- [RaBitQ](wiki/entities/rabitq.md) - Data-oblivious ANN quantization family extended here to multi-bit high-recall retrieval.
- [Low-Precision Quantization for KNN](wiki/entities/low-precision-quantization-knn.md) - Implementation-level int8 quantization approach for KNN indexes and distance kernels.
- [Norm-Explicit Quantization](wiki/entities/norm-explicit-quantization.md) - MIPS quantization paradigm that explicitly quantizes vector norms separately from directions.
- [Quantization to Speedup ANN](wiki/entities/quantization-to-speedup-ann.md) - IVF-HNSW acceleration paper using adaptive nprobe and per-centroid graph search.
- [SymphonyQG](wiki/entities/symphonyqg.md) - Graph-plus-quantization method combining RaBitQ, FastScan, implicit reranking, and graph refinement.
- [AQR-HNSW](wiki/entities/aqr-hnsw.md) - Low-confidence preprint on density-aware scalar quantization and progressive reranking for HNSW.
- [HNSW-LAVQ](wiki/entities/hnsw-lavq.md) - Low-confidence OpenReview paper on percentile-clipped scalar quantization and AVX2 HNSW kernels.
- [Information-Theoretic Binarization for Vector Search](wiki/entities/information-theoretic-binarization-vector-search.md) - Low-confidence preprint on binary quantization as a vector-search architecture.
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
- [FastLanes](wiki/entities/fastlanes.md) - SIMD-friendly compression layout project for high-throughput integer decoding and vectorized execution.
- [SIMD Investments](wiki/entities/simd-investments.md) - AVX-512 compiled-query pipeline paper on lane refill and control-flow divergence.
- [Compiled and Vectorized Queries](wiki/entities/compiled-vectorized-queries.md) - PVLDB 2018 execution-model comparison of vectorized and data-centric compiled queries.
- [Rethinking SIMD Vectorization](wiki/entities/rethinking-simd-vectorization.md) - SIGMOD 2015 database operator SIMD design paper covering gathers, scatters, hashing, and partitioning.
- [SIMD-Based Posting List Decoding](wiki/entities/simd-based-posting-list-decoding.md) - SIMD compressed posting-list decoding source for search-system ID/list paths.
- [SIMD Compression and Intersection](wiki/entities/simd-compression-and-intersection.md) - SIMD integer compression and sorted-list intersection source for filtered/vector hybrid search.
- [Stream VByte](wiki/entities/stream-vbyte.md) - SIMD-friendly byte-oriented integer codec with separate control and data streams.
- [Patience in Proximity](wiki/entities/patience-in-proximity.md) - Saturation-based early termination heuristic for HNSW traversal.
- [ANSMET](wiki/entities/ansmet.md) - Near-memory ANN acceleration with hybrid early termination.
- [CXLfork](wiki/entities/cxlfork.md) - CXL-based remote process cloning interface for serverless-style workloads.
- [DeltaPQ](wiki/entities/deltapq.md) - Lossless PQ code compression via MST-based differential encoding; 2–5× compression, direct search on compressed data.
- [HM-ANN](wiki/entities/hm-ann.md) - Heterogeneous memory (DRAM+PMM) graph ANNS for billion-scale search without compression.
- [LEANN](wiki/entities/leann.md) - Storage-efficient vector index using on-the-fly embedding recomputation and high-degree-preserving graph pruning.
- [LEP](wiki/entities/lep.md) - Lossy variant of ALP for vector embeddings; tunable compression via per-dimension decimal encoding.
- [Lossless Compression of Vector IDs](wiki/entities/lossless-compression-of-vector-ids.md) - ANS-based lossless compression of IVF list IDs and graph edges; up to 7× ID compression.
- [PipeANN](wiki/entities/pipeann.md) - Low-latency on-disk graph ANNS aligning best-first search with SSD I/O via asynchronous pipeline.
- [PolarCXLMem](wiki/entities/polarcxlmem.md) - CXL-switch-based disaggregated-memory architecture for cloud-native databases.
- [SegPQ](wiki/entities/segpq.md) - Lossless PQ codebook compression via ε-PLA; up to 4.7× reduction, SIMD-aware query processing.
- [TriBase](wiki/entities/tribase.md) - Cluster-based ANNS pruning via distance and angle triangle inequalities; >99.4% pruning rate.
- [XNC](wiki/entities/xnc.md) - Hardware-friendly lossless compression for LLM FP16 embedding layers; 1.34× compression, 4/3 cycle encode/decode.
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
- [Cache Locality is Not Enough: PQ Fast Scan](wiki/source-notes/pq-fast-scan-2015.md) - Source note on SIMD in-register lookup for accelerating PQ/ADC ANN scan.
- [Quicker ADC](wiki/source-notes/quicker-adc-2019.md) - Source note on AVX2/AVX-512 ADC acceleration for product quantization in FAISS and IVF-HNSW settings.
- [Low-Precision Quantization for Efficient Nearest Neighbor Search](wiki/source-notes/low-precision-quantization-knn-2021.md) - Source note on int8 low-precision KNN storage and distance computation.
- [Norm-Explicit Quantization](wiki/source-notes/norm-explicit-quantization-mips-2020.md) - Source note on norm/direction decomposition for MIPS quantization.
- [Quantization to Speedup Approximate Nearest Neighbor Search](wiki/source-notes/quantization-to-speedup-ann-2024.md) - Source note on IVF-HNSW adaptive termination and per-centroid graph search.
- [SymphonyQG](wiki/source-notes/symphonyqg-2024.md) - Source note on graph-plus-RaBitQ/FastScan co-design for ANN search.
- [AQR-HNSW](wiki/source-notes/aqr-hnsw-2026.md) - Low-confidence source note on density-aware quantized HNSW and progressive reranking.
- [Quantization-Enhanced HNSW / LAVQ](wiki/source-notes/quantization-enhanced-hnsw-lavq-2025.md) - Low-confidence source note on percentile-clipped scalar quantization for HNSW.
- [Information-Theoretic Binarization for Vector Search](wiki/source-notes/information-theoretic-binarization-vector-search-2026.md) - Low-confidence source note on binary vector-search architecture.
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
- [Gorilla: A Fast, Scalable, In-Memory Time Series Database](wiki/source-notes/gorilla-2015.md) - Pelkonen et al. VLDB 2015; foundational XOR-based streaming float compression (Facebook).
- [Chimp: Efficient Lossless Floating Point Compression for Time Series Databases](wiki/source-notes/chimp-2022.md) - Liakos et al. PVLDB 2022; improved XOR encoding with leading-zero analysis and Chimp128 multi-predecessor variant.
- [Elf: Erasing-based Lossless Floating-Point Compression](wiki/source-notes/elf-2023.md) - Li et al. PVLDB 2023; trailing-zero creation via mantissa erasure with lossless restoration.
- [ALP: Adaptive Lossless floating-Point Compression](wiki/source-notes/alp-2023.md) - Afroozeh et al. SIGMOD 2023; vectorized adaptive encoding (decimal→int or front-bits), SIMD-friendly.
- [Camel: Efficient Compression of Floating-Point Time Series](wiki/source-notes/camel-2024.md) - Yao et al. SIGMOD 2024; integer-decimal separation with stable-trailing-zero decimal XOR.
- [DeXOR: Enabling XOR in Decimal Space for Streaming Lossless Compression](wiki/source-notes/dexor-2025.md) - Lv et al. arXiv 2025; decimal XOR with longest common prefix extraction and suffix scaling.
- [DeltaPQ: Lossless Product Quantization Code Compression](wiki/source-notes/deltapq-2020.md) - Wang and Deng PVLDB 2020; MST-based differential compression of PQ codes.
- [SegPQ: A Learned Approach to Compress PQ Codebooks](wiki/source-notes/segpq-2025.md) - Liu et al. PVLDB 2025; ε-PLA model for lossless codebook compression.
- [Rethinking Vector Embeddings Search for Analytical Database Systems (LEP)](wiki/source-notes/lep-2024.md) - Krippner Master's Thesis 2024; lossy ALP adaptation for vector embeddings.
- [Lossless Compression of Vector IDs for ANNS](wiki/source-notes/lossless-compression-of-vector-ids-for-anns-2025.md) - Severo et al. arXiv 2025; ANS and wavelet tree compression of auxiliary ANN data.
- [LEANN: A Low-Storage Overhead Vector Index](wiki/source-notes/leann-2025.md) - Wang et al. under review 2025; embedding recomputation with pruned graph index.
- [TriBase: A Vector Data Query Engine for Lossless Pruning via Triangle Inequalities](wiki/source-notes/tribase-2025.md) - Xu et al. SIGMOD 2025; fine-grained triangle inequality pruning in cluster-based ANN.
- [HM-ANN: Efficient Billion-Point Nearest Neighbor Search on Heterogeneous Memory](wiki/source-notes/hm-ann-2020.md) - Ren et al. NeurIPS 2020; memory-tier-aware HNSW generalization for DRAM+PMM.
- [PipeANN: Aligning Best-First Search with SSD for Low-Latency Graph ANNS](wiki/source-notes/pipeann-2025.md) - Guo and Lu OSDI 2025; asynchronous I/O pipeline for on-disk graph traversal.
- [XNC: XOR and NOT-Based Lossless Compression for LLM Embedding Layers](wiki/source-notes/xnc-2025.md) - Lee et al. ISCAS 2025; hardware-friendly FP16 embedding compression via XOR+NOT truncation.
- [Make the Most Out of Your SIMD Investments](wiki/source-notes/simd-investments-2020.md) - Source note on AVX-512 lane refill for control-flow divergence in compiled query pipelines.
- [The FastLanes Compression Layout](wiki/source-notes/fastlanes-2023.md) - Source note on SIMD-friendly compressed integer layout and scalar auto-vectorized decoding.
- [Everything You Always Wanted to Know About Compiled and Vectorized Queries But Were Afraid to Ask](wiki/source-notes/compiled-vectorized-queries-2018.md) - Source note on vectorized versus data-centric compiled execution.
- [Rethinking SIMD Vectorization for In-Memory Databases](wiki/source-notes/rethinking-simd-vectorization-2015.md) - Source note on SIMD gathers/scatters, selections, hash tables, partitioning, sorting, and joins.
- [SIMD-Based Decoding of Posting Lists](wiki/source-notes/simd-based-decoding-posting-lists-2011.md) - Source note on SIMD decoding for compressed posting lists.
- [SIMD Compression and the Intersection of Sorted Integers](wiki/source-notes/simd-compression-intersection-2014.md) - Source note on SIMD integer decoding and sorted-list intersection.
- [Stream VByte](wiki/source-notes/stream-vbyte-2017.md) - Source note on SIMD-friendly byte-oriented integer compression.

## Analyses

- [Billion-Scale Vector Search Literature Map](wiki/analyses/billion-scale-vector-search-literature-map.md) - Research map of covered and not-yet-ingested billion-scale vector search systems.
- [Graph-Theoretic ANN Inbox Candidates](wiki/analyses/graph-theoretic-ann-inbox-candidates.md) - Candidate queue and rationale for proximity/RNG/MRNG/small-world graph papers, now including the first ingested batch.
- [ANN Benchmarking And Graph Coverage Gaps](wiki/analyses/ann-benchmarking-and-graph-coverage-gaps.md) - Gap analysis and inbox candidate queue seeded by ANN-Benchmarks and the graph-based ANNS survey.
- [SIMD And Scalar Quantization Inbox Candidates](wiki/analyses/simd-scalar-quantization-inbox-candidates.md) - Ingested batch record for ANN SIMD/PQ scan papers and scalar/bit quantization papers.
- [ANNS Research Figure Design Guide](wiki/analyses/anns-research-figure-design-guide.md) - Figure-type taxonomy, representative screenshot atlas, and review workflow for ANNS research figures.
