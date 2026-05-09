---
id: anns-research-figure-design-guide
type: analysis
status: active
created: 2026-05-09
updated: 2026-05-09
tags:
  - ann
  - paper-writing
  - figures
  - systems
  - evaluation
source_count: 25
sources:
  - raw/sources/papers/ann-benchmarks-2018.pdf
  - raw/sources/papers/graph-based-anns-survey-2021.pdf
  - raw/sources/papers/hnsw-2016.pdf
  - raw/sources/papers/nsg-2019.pdf
  - raw/sources/papers/product-quantization-2011.pdf
  - raw/sources/papers/faiss-library-2025.pdf
  - raw/sources/papers/diskann-2019.pdf
  - raw/sources/papers/spann-2021.pdf
  - raw/sources/papers/starling-2024.pdf
  - raw/sources/papers/spfresh-2023.pdf
  - raw/sources/papers/bang-2024.pdf
  - raw/sources/papers/rummy-2024.pdf
  - raw/sources/papers/gustann-2025.pdf
  - raw/sources/papers/fusionanns-2025.pdf
  - raw/sources/papers/svfusion-2026.pdf
  - raw/sources/papers/smartanns-2024.pdf
  - raw/sources/papers/cxl-anns-2024.pdf
  - raw/sources/papers/cxl-memory-characterization-2023.pdf
  - raw/sources/papers/d-hnsw-2025.pdf
  - raw/sources/papers/vbase-2023.pdf
  - raw/sources/papers/rnsg-2026.pdf
  - raw/sources/papers/fanng-2016.pdf
  - raw/sources/papers/efanna-2016.pdf
  - raw/sources/papers/flann-2014.pdf
  - raw/sources/papers/falconn-lsh-angular-2015.pdf
related:
  - approximate-nearest-neighbor-search
  - ann-benchmarking-methodology
  - proximity-graph-theory-for-ann
  - second-tier-memory-for-vector-search
  - disaggregated-memory-vector-search
confidence: medium
---

# ANNS Research Figure Design Guide

## Purpose

This page summarizes how the ANNS papers in this vault communicate research ideas visually. It is meant for a drawing agent that must choose the right figure type, borrow visual patterns from nearby papers, and review the figure against ANNS-specific correctness and evaluation expectations.

Rendered page screenshots live in [wiki/assets/figure-guide](../assets/figure-guide/). They are page-level references, not extracted source artwork. Use them to inspect figure placement, caption style, density, and surrounding explanatory text.

## Main Split: System Figures vs Experiment Figures

System figures explain **mechanism**: what components exist, where data lives, what moves, and when each actor runs. Experiment figures support **evidence**: how recall, latency, throughput, memory, I/O, or update behavior changes under baselines, parameters, datasets, and hardware.

Do not merge these roles unless the paper explicitly needs a small callout. A good ANNS systems paper usually needs both:

- one or more mechanism figures: architecture, data layout, query workflow, update path, or graph/quantization intuition;
- several evidence figures: recall-QPS Pareto, recall-latency, throughput/latency, ablation, sensitivity, and breakdown.

## Figure Taxonomy

| Class | What it should show | Good references | Drawing notes |
|---|---|---|---|
| Problem/motivation figure | Why the baseline fails and what pressure exists. | CXL-ANNS characterization pages, SPFresh recall/tail-latency setup, FusionANNS motivation, Starling baseline-vs-proposed layout. | Best when it isolates one bottleneck: capacity, traffic, random I/O, stale updates, GPU underutilization, or filter selectivity. |
| System architecture | Components, tiers, software/hardware boundary, and major data/control arrows. | [CXL-ANNS p13](../assets/figure-guide/cxl-anns-p13-overview.png), [FusionANNS p8](../assets/figure-guide/fusionanns-p8-architecture-workflow.png), [BANG p4](../assets/figure-guide/bang-p4-schema.png), [GustANN p8](../assets/figure-guide/gustann-p8-architecture.png), [SVFusion p4](../assets/figure-guide/svfusion-p4-overview-index.png). | Use block diagrams with explicit actor labels. Separate host/device and memory tiers. Avoid ungrounded "fast path" arrows. |
| Data placement / physical layout | Which objects are stored where and how locality changes search. | [Starling p6](../assets/figure-guide/starling-p6-layout-search.png), [PQ p8](../assets/figure-guide/pq-p8-ivfadc-overview.png), SPFresh storage, CXL-ANNS placement, GustANN on-disk graph. | Good for SSD/CXL/GPU papers because placement is often the contribution. Use before/after panels. |
| Query workflow / pipeline | Query stages, batching, scheduling, deduplication, reranking, or prefetch. | [RUMMY p6](../assets/figure-guide/rummy-p6-overview-retrofit.png), [FusionANNS p8](../assets/figure-guide/fusionanns-p8-architecture-workflow.png), BANG workflow, GustANN GPU iteration, SVFusion streaming workflow. | Numbered arrows help. Mark synchronous vs asynchronous work and where exact distance is computed. |
| Algorithm intuition | Why search or pruning works. | [HNSW p3](../assets/figure-guide/hnsw-p3-hierarchy-neighbor-selection.png), [NSG p7](../assets/figure-guide/nsg-p7-edge-selection.png), graph survey base-graph and pipeline figures, PQ distance-computation figure. | Use small toy graphs or 2D examples. Avoid pretending high-dimensional behavior is exactly geometric. |
| Taxonomy / pipeline | Families, components, design space, or decision tree. | [Graph survey p7](../assets/figure-guide/graph-survey-p7-pipeline.png), FAISS decision-tree page, ANN-Benchmarks harness page. | Useful for related-work or background sections. Must not become a decorative survey poster. |
| Characterization | Hardware or workload facts motivating design. | [CXL memory p5](../assets/figure-guide/cxl-memory-p5-latency-bandwidth.png), CXL-ANNS baseline/slowdown/transfer pages, GustANN bandwidth pages, SVFusion access-pattern pages. | State workload, hardware, unit, and conclusion. Use simple plots, not complex multi-panel dashboards unless necessary. |
| Recall-QPS / recall-latency | Quality-speed frontier. | [ANN-Benchmarks p12](../assets/figure-guide/ann-benchmarks-p12-recall-qps.png), [SPANN p7](../assets/figure-guide/spann-p7-performance.png), [Starling p17](../assets/figure-guide/starling-p17-recall-latency-qps.png), [BANG p11](../assets/figure-guide/bang-p11-throughput-recall.png). | For ANN claims this is the default evidence shape. Label recall@k and axis direction. |
| Ablation / breakdown | Which design component caused the gain. | FusionANNS technology breakdown, SVFusion framework/deletion/placement effects, Starling block-search optimizations, RUMMY reordering/grouping, CXL-ANNS transfer/cache/interface breakdowns. | Use one plot per mechanism family. Avoid "all knobs at once" if the claim is causal. |
| Sensitivity / scalability | Effect of parameter, thread count, batch size, memory ratio, device count, k, graph degree, candidate set, or dataset size. | Starling parameter pages, CXL-ANNS device/host scaling, SVFusion GPU memory ratio and thread count, SmartANNS SmartSSD scaling, GustANN SSD count. | Show monotonic/non-monotonic trend and chosen default. |
| Dynamic workload / update figure | Time, freshness, deletes/inserts, recall drift, tail latency. | SPFresh daily traces, SVFusion streaming workloads, VBASE vector-relational selectivity. | Must show time or update mix. Static recall-QPS is not enough. |

## System Figure Patterns

### 1. Architecture Overview

The strongest architecture figures expose a boundary:

- host vs device: CXL-ANNS, BANG, SmartANNS;
- CPU vs GPU vs SSD: GustANN, FusionANNS, SVFusion;
- memory vs disk segment: Starling, SPFresh;
- database engine vs vector index: VBASE, Milvus.

The figure should answer: **who owns state, who computes, who moves data, and what is new?** If a block has no state or action, remove it.

### 2. Data Layout And Placement

ANNS system contributions often depend on where vectors, graph edges, PQ/SQ codes, centroids, metadata, query state, or caches live. Starling's layout figure is a strong pattern: it uses side-by-side panels to compare baseline physical layout with proposed reordered layout and search strategy.

Use this pattern for any claim about fewer SSD reads, lower PCIe/CXL traffic, better locality, or hot/cold placement.

### 3. Query Execution Workflow

Workflow figures should be stage diagrams, not architecture maps. BANG, GustANN, FusionANNS, RUMMY, and SVFusion all use staged arrows to show batching, GPU kernels, transfer, deduplication, scoring, or reranking.

Review questions:

- Which stage reads approximate codes?
- Which stage reads raw vectors?
- Where are candidates generated and merged?
- What is batched, prefetched, cached, deduplicated, or reranked?
- What is on the critical path?

### 4. Algorithm And Graph Intuition

HNSW, NSG, FANNG, SSG, RNSG, and PQ-style papers use small toy examples to explain graph layers, neighbor selection, occlusion, monotonic paths, or asymmetric distance computation. These figures are useful for method sections, but should be honest: a 2D toy graph explains a rule, not the full high-dimensional behavior.

## Experiment Figure Patterns

### 1. Pareto Curves

ANN-Benchmarks establishes the core convention: recall on one axis, QPS or latency on the other, and the frontier interpretation in the caption. For systems papers, preserve the matched-recall discipline even when the plot is throughput vs recall or latency vs recall.

Use this when comparing algorithms or systems under tunable search parameters.

### 2. Throughput And Latency Bars

SPANN, FusionANNS, CXL-ANNS, SmartANNS, and SVFusion use grouped bars for QPS, latency, or normalized improvement. Bars are useful when the recall is fixed in the caption or plot title. If recall is not fixed, a bar chart can mislead.

### 3. Breakdown Figures

Breakdowns are essential for systems claims. CXL-ANNS uses transfer, cache, interface, distance-computation, and utilization plots; Starling breaks down search time and I/O effects; FusionANNS and SVFusion show mechanism-level improvements.

Use a breakdown when claiming a design component matters.

### 4. Sensitivity And Scaling

Thread count, batch size, device count, memory ratio, graph degree, candidate size, rerank budget, SSD count, and vector dimension all affect ANNS systems. A good sensitivity figure shows both the chosen default and the failure mode when the parameter changes.

### 5. Dynamic And Streaming Experiments

Static recall-QPS plots are insufficient for update-heavy papers. SPFresh and SVFusion use update throughput, latency percentiles, miss rate, and recall drift style metrics. If the system claims freshness, use dynamic figures.

## Screenshot Atlas

| Screenshot | Source PDF page | Use this for |
|---|---:|---|
| [Graph survey pipeline](../assets/figure-guide/graph-survey-p7-pipeline.png) | Graph-Based ANNS Survey p7 | Component taxonomy: construction, seed, routing, connectivity. |
| [HNSW hierarchy](../assets/figure-guide/hnsw-p3-hierarchy-neighbor-selection.png) | HNSW p3 | Layered graph and neighbor-selection intuition. |
| [NSG edge selection](../assets/figure-guide/nsg-p7-edge-selection.png) | NSG p7 | MRNG/monotonic path and edge-pruning intuition. |
| [PQ IVFADC overview](../assets/figure-guide/pq-p8-ivfadc-overview.png) | PQ p8 | Quantized index layout and approximate distance workflow. |
| [FAISS decision tree](../assets/figure-guide/faiss-p23-index-decision-tree.png) | FAISS library p23 | Index-selection taxonomy and library hierarchy style. |
| [CXL-ANNS overview](../assets/figure-guide/cxl-anns-p13-overview.png) | CXL-ANNS p13 | Hardware/software boundary and CXL endpoint architecture. |
| [FusionANNS architecture](../assets/figure-guide/fusionanns-p8-architecture-workflow.png) | FusionANNS p8 | CPU/GPU/SSD workflow with multi-tier indexing. |
| [BANG schema](../assets/figure-guide/bang-p4-schema.png) | BANG p4 | GPU-centric graph search with host-resident graph access. |
| [GustANN architecture](../assets/figure-guide/gustann-p8-architecture.png) | GustANN p8 | GPU traversal with CPU-assisted SSD transfer. |
| [Starling layout](../assets/figure-guide/starling-p6-layout-search.png) | Starling p6 | Baseline-vs-proposed disk graph layout and search strategy. |
| [SVFusion overview](../assets/figure-guide/svfusion-p4-overview-index.png) | SVFusion p4 | Streaming CPU-GPU-disk index hierarchy and workflow. |
| [RUMMY overview](../assets/figure-guide/rummy-p6-overview-retrofit.png) | RUMMY p6 | Batch query plan retrofitting and GPU pipelining. |
| [SPFresh architecture](../assets/figure-guide/spfresh-p7-architecture.png) | SPFresh p7 | Fresh-update architecture and local rebuild workflow. |
| [SmartANNS architecture](../assets/figure-guide/smartanns-p6-architecture.png) | SmartANNS p6 | SmartSSD/NDP architecture and shard-level execution. |
| [CXL memory characterization](../assets/figure-guide/cxl-memory-p5-latency-bandwidth.png) | CXL memory p5 | Hardware latency/bandwidth characterization style. |
| [ANN-Benchmarks recall-QPS](../assets/figure-guide/ann-benchmarks-p12-recall-qps.png) | ANN-Benchmarks p12 | Standard recall-QPS Pareto plotting style. |
| [CXL-ANNS results](../assets/figure-guide/cxl-anns-p19-qps-recall-latency.png) | CXL-ANNS p19 | QPS, recall-QPS, and single-query latency panels. |
| [Starling results](../assets/figure-guide/starling-p17-recall-latency-qps.png) | Starling p17 | Recall-latency and QPS-recall for disk-resident graph indexes. |
| [SVFusion results](../assets/figure-guide/svfusion-p10-throughput-latency.png) | SVFusion p10 | Streaming workload throughput, recall, miss rate, and latency. |
| [GustANN results](../assets/figure-guide/gustann-p17-search-results.png) | GustANN p17 | Multi-dataset search-result panel for SSD+GPU systems. |
| [SPANN performance](../assets/figure-guide/spann-p7-performance.png) | SPANN p7 | DiskANN/SPANN billion-scale performance comparison. |
| [BANG throughput-recall](../assets/figure-guide/bang-p11-throughput-recall.png) | BANG p11 | GPU graph ANN throughput vs recall curves. |
| [FusionANNS results](../assets/figure-guide/fusionanns-p11-throughput-latency.png) | FusionANNS p11 | QPS/latency under fixed and varied accuracy levels. |
| [RUMMY performance](../assets/figure-guide/rummy-p11-performance.png) | RUMMY p11 | GPU/CPU comparison and accuracy-level plots. |
| [SmartANNS results](../assets/figure-guide/smartanns-p11-throughput-recall.png) | SmartANNS p11 | NDP throughput, recall-QPS, energy, and scaling panels. |

## Figure Design Recipes

### For a new system architecture figure

1. Define the one claim: placement, scheduling, caching, batching, acceleration, or update handling.
2. Pick one primary visual grammar: tiered blocks, pipeline stages, or side-by-side baseline/proposed.
3. Label state and data structures, not just component names.
4. Show data movement volume directionally if exact numbers belong in experiments.
5. Add a caption that states the design invariant: what stays local, what moves, what is batched, or what is reranked.

### For a new experiment figure

1. Fix the comparison question: speed-quality frontier, throughput at fixed recall, memory/index cost, update freshness, or data movement.
2. Choose the chart type:
   - Pareto curve for algorithm/system tradeoff.
   - Grouped bar for fixed-quality comparison.
   - Stacked bar for breakdown.
   - Line plot for sensitivity or time-varying workload.
   - Scatter for implementation landscape.
3. Put recall@k, dataset, and units where a reviewer cannot miss them.
4. Keep legends short and consistent across the paper.
5. Do not use one dataset or one parameter setting to imply a universal result.

## Review Checklist For A Figure Agent

### System Figure

- Does it expose the new mechanism rather than generic infrastructure?
- Are all tiers and actors named?
- Are arrows directional and meaningful?
- Are offline build, online query, update, and background work separated if needed?
- Does the figure match the implementation context?
- Can the reader infer the ablation that should test this mechanism?

### Experiment Figure

- Is recall or approximation quality defined?
- Are baseline implementations and parameter settings identified?
- Are units and log scales visible?
- Does the plot support the written claim at matched recall?
- Is hardware placement or memory tier part of the label when relevant?
- Are dynamic/update claims backed by time, freshness, or repair metrics?

### Caption

- First sentence: what the figure is.
- Second sentence: what the reader should learn.
- If needed: key controlled condition, such as fixed recall, dataset, k, thread count, memory budget, or hardware tier.

## Common Failure Modes

- A system figure shows boxes but not state, movement, or decision rules.
- A workflow figure omits where exact distance or rerank happens.
- A graph intuition figure looks exact but only describes a heuristic.
- A benchmark figure compares QPS without recall alignment.
- A hardware figure omits the bottleneck resource, such as PCIe, SSD I/O, CXL bandwidth, GPU HBM, or CPU threads.
- An ablation figure uses labels that do not correspond to named mechanisms in the method section.
- A dynamic-update paper only shows static recall-QPS.

## Open Maintenance Tasks

- Add cropped single-figure assets if future drawing agents need finer-grained visual references.
- Add CAGRA, FreshDiskANN, BigANN/streaming benchmark figures after those sources are ingested.
- Add paper-local figure templates when a concrete target paper repository is available.
