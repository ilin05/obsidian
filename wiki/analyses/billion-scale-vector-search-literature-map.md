---
id: billion-scale-vector-search-literature-map
type: analysis
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - billion-scale
  - literature-map
source_count: 13
sources:
  - raw/sources/papers/gustann-2025.pdf
  - raw/sources/papers/fusionanns-2025.pdf
  - raw/sources/papers/rummy-2024.pdf
  - raw/sources/papers/smartanns-2024.pdf
  - raw/sources/papers/vbase-2023.pdf
  - raw/sources/papers/starling-2024.pdf
  - raw/sources/papers/bang-2024.pdf
  - raw/sources/papers/spfresh-2023.pdf
  - raw/sources/papers/svfusion-2026.pdf
  - https://experts.illinois.edu/en/publications/grip-multi-store-capacity-optimized-high-performance-nearest-neig/
  - https://idsc.miami.edu/idec-indexable-distance-estimating-codes-for-approximate-nearest-neighbor-search/
  - https://conf.researchr.org/details/PPoPP-2024/PPoPP-2024-papers/9/ParANN-Scalable-and-Deterministic-Parallel-Graph-Based-Approximate-Nearest-Neighbor-
  - https://ppopp23.sigplan.org/details/PPoPP-2023-papers/24/iQAN-Fast-and-Accurate-Vector-Search-with-Efficient-Intra-Query-Parallelism-on-Multi
related:
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
  - gustann
  - fusionanns
  - rummy
  - smartanns
  - vbase
  - starling
  - bang
  - spfresh
  - svfusion
  - diskann
  - spann
confidence: medium
---

# Billion-Scale Vector Search Literature Map

## Scope

This page is a research map, not a full ingest of every listed paper. It was created after ingesting [GustANN](../source-notes/gustann-2025.md) and [FusionANNS](../source-notes/fusionanns-2025.md), then expanded by downloading and ingesting six high-value gap papers: [RUMMY](../source-notes/rummy-2024.md), [SmartANNS](../source-notes/smartanns-2024.md), [VBASE](../source-notes/vbase-2023.md), [Starling](../source-notes/starling-2024.md), [BANG](../source-notes/bang-2024.md), and [SPFresh](../source-notes/spfresh-2023.md). It now also includes [SVFusion](../source-notes/svfusion-2026.md) as an emerging CPU-GPU-disk streaming vector search system.

The main lens is systems research for billion-scale vector search: how indexes, memory tiers, GPUs, SSDs, and update/query workloads are co-designed when raw DRAM is too expensive.

## Already Covered In This Vault

| Area | Existing pages | Role |
|---|---|---|
| Graph foundations | [HNSW](../entities/hnsw.md), [NSG](../entities/nsg.md), [DiskANN](../entities/diskann.md) | Baseline graph traversal and SSD-resident graph design. |
| Cluster/IVF systems | [SPANN](../entities/spann.md), [FAISS](../entities/faiss.md), [ScaNN](../entities/scann.md) | Partitioning, quantization, and library-level baselines. |
| Quantization | [Product Quantization](../entities/product-quantization.md), [RaBitQ](../entities/rabitq.md), [TurboQuant](../entities/turboquant.md) | Compression and distance-estimation layer. |
| Second-tier/disaggregated memory | [CXL-ANNS](../entities/cxl-anns.md), [d-HNSW](../entities/d-hnsw.md), [ANSMET](../entities/ansmet.md) | Memory-tier-aware ANN serving and acceleration. |
| Host-memory/GPU and SSD+GPU systems | [RUMMY](../entities/rummy.md), [BANG](../entities/bang.md), [SVFusion](../entities/svfusion.md), [GustANN](../entities/gustann.md), [FusionANNS](../entities/fusionanns.md) | Adjacent systems that optimize GPU/CPU/SSD data movement. |
| Segment, NDP, query semantics, and freshness | [Starling](../entities/starling.md), [SmartANNS](../entities/smartanns.md), [VBASE](../entities/vbase.md), [SPFresh](../entities/spfresh.md) | Coverage for data-segment layout, computational storage, vector-relational queries, and online updates. |

## Newly Ingested Papers

| Paper | System | Design family | Why it matters here |
|---|---|---|---|
| [GustANN](../source-notes/gustann-2025.md) | GustANN | SSD-resident graph + GPU traversal + CPU-assisted transfer | Shows that high-throughput on-SSD graph ANNS is limited by CPU and PCIe transfer unless query state, transfer granularity, and SSD balance are co-designed. |
| [FusionANNS](../source-notes/fusionanns-2025.md) | FusionANNS | SSD hierarchical index + GPU PQ filtering + selective rerank | Useful example of a compressed fast path followed by selective expensive raw-vector access. |
| [RUMMY](../source-notes/rummy-2024.md) | RUMMY | IVF + host memory + GPU reordered pipelining | Directly relevant to batched ANN scheduling and avoiding redundant CPU-GPU transfer across queries. |
| [SmartANNS](../source-notes/smartanns-2024.md) | SmartANNS | Host CPU + SmartSSD cooperative hierarchy | Important near-data-processing contrast to commodity host-side SSD and CXL designs. |
| [Starling](../source-notes/starling-2024.md) | Starling | Disk-resident graph layout for vector database segments | Key SSD-graph layout comparison beyond DiskANN; focuses on I/O efficiency under per-segment budgets. |
| [BANG](../source-notes/bang-2024.md) | BANG | Graph ANNS on one GPU with host-resident graph and compressed GPU vectors | Relevant GPU graph baseline for billion-scale datasets that exceed GPU memory. |
| [SPFresh](../source-notes/spfresh-2023.md) | SPFresh | In-place fresh updates for disk-based vector search | Important for systems that claim online update support rather than read-mostly serving. |
| [VBASE](../source-notes/vbase-2023.md) | VBASE | Vector-relational query processing through relaxed monotonicity | Important for vector database query semantics and filtered/complex queries. |
| [SVFusion](../source-notes/svfusion-2026.md) | SVFusion | CPU-GPU-disk graph ANNS for streaming search/insert/delete | Connects out-of-GPU-memory execution with update freshness and cross-tier consistency. |

## Relevant Papers Not Yet Ingested

| Paper | Venue/source | Main idea | Why it belongs in the vault |
|---|---|---|---|
| [GRIP: Multi-Store Capacity-Optimized High-Performance Nearest Neighbor Search](https://experts.illinois.edu/en/publications/grip-multi-store-capacity-optimized-high-performance-nearest-neig/) | CIKM 2019 | Hybrid DRAM+SSD ANN algorithm and cost model for capacity-optimized vector search. | Historical precursor to modern SSD/CXL capacity-tier arguments; useful for the related-work arc before DiskANN/SPANN. |
| [iDEC: Indexable Distance Estimating Codes for Approximate Nearest Neighbor Search](https://idsc.miami.edu/idec-indexable-distance-estimating-codes-for-approximate-nearest-neighbor-search/) | PVLDB 2020 | Extends LSH with indexable distance-estimating codes; reports low index space and external-memory applicability for Hamming/edit distance. | Useful for broad external-memory ANN and theoretical index families outside graph/IVF. |
| [ParANN / ParlayANN: Scalable and Deterministic Parallel Graph-Based ANNS](https://conf.researchr.org/details/PPoPP-2024/PPoPP-2024-papers/9/ParANN-Scalable-and-Deterministic-Parallel-Graph-Based-Approximate-Nearest-Neighbor-) | PPoPP 2024 | Deterministic parallel implementations of graph-based ANNS algorithms at billion scale. | Relevant to build-time/index-construction parallelism and large-scale graph-system methodology. |
| [iQAN: Fast and Accurate Vector Search with Efficient Intra-Query Parallelism](https://ppopp23.sigplan.org/details/PPoPP-2023-papers/24/iQAN-Fast-and-Accurate-Vector-Search-with-Efficient-Intra-Query-Parallelism-on-Multi) | PPoPP 2023 | Optimizes intra-query parallel graph search on multicore CPUs by improving convergence, reducing redundancy, and mitigating synchronization overhead. | Useful for understanding graph-search parallelism within a single query versus across concurrent queries. |
| CAGRA | ICDE 2024 | GPU-parallel graph construction and approximate nearest-neighbor search. | Static GPU graph primitive extended by SVFusion; should be ingested before treating GPU graph baselines as complete. |
| FreshDiskANN | arXiv 2021 | Fast and accurate graph-based ANN index for streaming similarity search. | Direct baseline for dynamic graph updates and streaming freshness. |

## System Design Axes

| Axis | Representative papers | What to watch |
|---|---|---|
| SSD-resident graph traversal | DiskANN, Starling, GustANN | Page granularity, graph layout, SSD queue depth, entry-point imbalance. |
| GPU beyond HBM | RUMMY, BANG | Where compressed vectors live, what crosses PCIe, and whether batching is latency-tolerable. |
| SSD + GPU collaborative search | FusionANNS, GustANN | Whether GPU helps filtering, graph traversal, reranking, or I/O scheduling. |
| CPU-GPU-disk streaming | SVFusion, SPFresh, FreshDiskANN | Update consistency, cache residency, repair/rebuild cost, and recall drift. |
| Near-data / specialized storage | SmartANNS, ANSMET | Whether gains rely on non-commodity hardware or endpoint compute. |
| CXL / disaggregated memory | CXL-ANNS, d-HNSW | Whether random access is scheduled, cached, coalesced, or offloaded. |
| Dynamic vector data | SPFresh, VBASE, Milvus | Update semantics, filtered queries, and integration with database operators. |
| Parallel graph execution | iQAN, ParANN, HNSW/NSG variants | Intra-query versus inter-query parallelism, determinism, and synchronization cost. |

## Implications For Future Work

The external literature reinforces a general systems framing: the central bottleneck in billion-scale ANN is often not the abstract ANN algorithm alone, but the movement and scheduling of candidate vectors across a slower or narrower tier.

For future paper positioning:

- Treat GustANN, FusionANNS, RUMMY, BANG, SmartANNS, and Starling as adjacent related work on heterogeneous and SSD-based billion-scale serving.
- Treat SVFusion as the emerging bridge between GPU/out-of-HBM search and streaming update freshness.
- Be explicit when a design does or does not rely on hardware offload, SmartSSD processing, or GPU acceleration.
- Preserve the read-heavy serving boundary unless SPFresh-style update behavior is implemented and evaluated.
- Include VBASE when claims involve filtered, relational, or SQL-integrated vector queries rather than pure TopK.

## Suggested Next Ingest Queue

- Parallel graph-search related work: iQAN, ParANN.
- Historical/external-memory context: GRIP, iDEC.
- GPU/streaming graph systems: CAGRA, FreshDiskANN, GGNN, SONG, GANNS, PilotANN.

## Open Questions

- Which systems should be direct baselines versus related work only? GustANN and FusionANNS are hardware-adjacent but not CXL-equivalent.
- When should batching/coalescing ideas from SSD+GPU systems transfer to CXL or RDMA memory tiers?
- Should a future paper include a short taxonomy figure: DRAM-only, SSD-only, SSD+GPU, SmartSSD/NDP, CXL, RDMA-disaggregated?
