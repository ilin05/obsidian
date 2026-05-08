---
id: ann-benchmarking-and-graph-coverage-gaps
type: analysis
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - literature-map
  - benchmarking
  - graph-index
source_count: 11
sources:
  - raw/sources/papers/ann-benchmarks-2018.pdf
  - raw/sources/papers/graph-based-anns-survey-2021.pdf
  - raw/sources/papers/svfusion-2026.pdf
  - raw/sources/papers/nn-descent-2011.pdf
  - raw/sources/papers/efanna-2016.pdf
  - raw/sources/papers/fanng-2016.pdf
  - raw/sources/papers/dpg-ann-experiments-2016.pdf
  - raw/sources/papers/ngt-onng-2018.pdf
  - raw/sources/papers/falconn-lsh-angular-2015.pdf
  - raw/sources/papers/flann-2014.pdf
  - raw/sources/papers/gnns-knn-graph-2011.pdf
related:
  - ann-benchmarking-methodology
  - approximate-nearest-neighbor-search
  - proximity-graph-theory-for-ann
  - ann-benchmarks
  - svfusion
  - nn-descent
  - efanna
  - fanng
  - diversified-proximity-graph
  - ngt-onng
  - flann
  - falconn
confidence: medium
---

# ANN Benchmarking And Graph Coverage Gaps

## Scope

This analysis uses [ANN-Benchmarks](../source-notes/ann-benchmarks-2018.md), [Graph-Based ANNS Survey 2021](../source-notes/graph-based-anns-survey-2021.md), and [SVFusion](../source-notes/svfusion-2026.md) as seed papers to identify what the ANN knowledge base still lacks. The 2026-05-08 inbox download pass was scoped to the user's requested pair: [ANN-Benchmarks](../entities/ann-benchmarks.md) and `raw/sources/papers/graph-based-anns-survey-2021.pdf`.

The goal is not to ingest every referenced paper immediately. It is to define a higher-value queue so future ingests improve coverage instead of adding isolated notes.

## Current Coverage

| Area | Covered pages | Remaining gap |
|---|---|---|
| High-recall graph baselines | [HNSW](../entities/hnsw.md), [NSG](../entities/nsg.md), [DiskANN](../entities/diskann.md), [NN-Descent](../entities/nn-descent.md), [EFANNA](../entities/efanna.md), [FANNG](../entities/fanng.md), [DPG](../entities/diversified-proximity-graph.md), [NGT/ONNG](../entities/ngt-onng.md) | SPTAG, HCNNG, PANNG, and IEH need better source coverage. |
| Proximity-graph theory | [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md), MRNG, SSG, RNSG | Classic RNG/Delaunay/MST/Voronoi references remain only partially covered. |
| In-memory benchmarking | [ANN-Benchmarks](../entities/ann-benchmarks.md), [ANN Benchmarking Methodology](../topics/ann-benchmarking-methodology.md), [FLANN](../entities/flann.md), [FALCONN](../entities/falconn.md) | Annoy and PyNNDescent remain library/baseline gaps if non-graph benchmark coverage becomes a focus. |
| Hardware-aware systems | DiskANN, SPANN, RUMMY, BANG, GustANN, FusionANNS, SmartANNS, SPFresh, CXL-ANNS, d-HNSW | GPU graph lineage and streaming update benchmarks are still thin. |
| Vector DB semantics | Milvus, VBASE | More filtered/structured ANN papers are needed if the vault supports database-facing claims. |

## Ingested From Inbox On 2026-05-08

These files were selected because they are cited by ANN-Benchmarks and/or Graph-Based ANNS Survey 2021, were not already represented as source notes/raw PDFs, and fill a specific coverage gap. They have now been ingested and moved from `raw/inbox/` to `raw/sources/papers/`.

| Source file | Paper | Seed source | Why it belongs |
|---|---|---|---|
| `raw/sources/papers/nn-descent-2011.pdf` | Dong, Charikar, and Li, "Efficient K-Nearest Neighbor Graph Construction for Generic Similarity Measures" | ANN-Benchmarks; graph survey | Original NN-Descent/KGraph construction paper; needed before discussing KGraph, NSG/DPG/NSSG initialization, or kNN graph construction cost. |
| `raw/sources/papers/gnns-knn-graph-2011.pdf` | Hajebi et al., "Fast Approximate Nearest-Neighbor Search with k-Nearest Neighbor Graph" | graph survey | Early GNNS-style graph traversal paper; clarifies the random-entry hill-climbing ancestor of later graph ANN methods. |
| `raw/sources/papers/efanna-2016.pdf` | Fu and Cai, "EFANNA: An Extremely Fast Approximate Nearest Neighbor Search Algorithm Based on kNN Graph" | graph survey | Important tree-initialized kNN graph method; useful for separating initialization from routing and pruning. |
| `raw/sources/papers/fanng-2016.pdf` | Harwood and Drummond, "FANNG: Fast Approximate Nearest Neighbour Graphs" | graph survey | RNG/occlusion-rule pruning branch that contrasts with HNSW/NSG neighbor diversification. |
| `raw/sources/papers/dpg-ann-experiments-2016.pdf` | Li et al., "Approximate Nearest Neighbor Search on High Dimensional Data - Experiments, Analyses, and Improvement" | graph survey | Broad empirical comparison plus DPG-style diversified graph improvement; bridges benchmarking and graph pruning. |
| `raw/sources/papers/ngt-onng-2018.pdf` | Iwasaki and Miyazaki, "Optimization of Indexing Based on k-Nearest Neighbor Graph for Proximity Search in High-dimensional Data" | graph survey | NGT/ONNG optimization paper; covers degree adjustment, path adjustment, and practical graph library behavior. |
| `raw/sources/papers/flann-2014.pdf` | Muja and Lowe, "Scalable Nearest Neighbor Algorithms for High Dimensional Data" | ANN-Benchmarks | Canonical FLANN paper; important non-graph auto-tuning/tree baseline. |
| `raw/sources/papers/falconn-lsh-angular-2015.pdf` | Andoni et al., "Practical and Optimal LSH for Angular Distance" | ANN-Benchmarks | FALCONN/LSH baseline; keeps the knowledge base from becoming graph-only. |

## Candidate Queue From ANN-Benchmarks

| Priority | Candidate | Why it matters |
|---|---|---|
| P0 | KGraph / NN-Descent | Core kNN-graph construction method; appears in ANN-Benchmarks and underlies NSG/DPG/NSSG-style initialization. Ingested as `raw/sources/papers/nn-descent-2011.pdf`. |
| P0 | NGT / PANNG | Production graph library and pruned bi-directed KNN graph baseline; important bridge between benchmark tooling and graph survey taxonomy. ONNG follow-up ingested as `raw/sources/papers/ngt-onng-2018.pdf`; original PANNG/SISAP paper still needs a stable PDF. |
| P1 | Annoy | Widely used tree/random-projection baseline; useful for explaining when non-graph methods remain competitive. |
| P1 | FLANN | Classic auto-tuning tree/hash ANN baseline; important historical comparison for benchmark methodology. Ingested as `raw/sources/papers/flann-2014.pdf`. |
| P1 | FALCONN / practical LSH | Strong LSH baseline and contrast to graph methods under angular distance. Ingested as `raw/sources/papers/falconn-lsh-angular-2015.pdf`. |
| P1 | PyNNDescent | Modern NN-Descent implementation that appears in ANN-Benchmarks-style evaluations. |

## Candidate Queue From Graph-Based ANNS Survey 2021

| Priority | Candidate | Graph family | Why it matters |
|---|---|---|---|
| P0 | KGraph / NN-Descent | KNNG | Construction primitive and baseline for refinement-based graph ANN. Ingested as `raw/sources/papers/nn-descent-2011.pdf`. |
| P0 | SPTAG | KNNG + tree seed acquisition | Microsoft production library and SPANN dependency. |
| P0 | NGT / ONNG | DG/KNNG/RNG hybrid | Practical graph library with path adjustment and VP-tree seed acquisition. Ingested as `raw/sources/papers/ngt-onng-2018.pdf`. |
| P1 | EFANNA | KNNG | Tree-initialized fast kNN graph construction; helps explain initialization effects. Ingested as `raw/sources/papers/efanna-2016.pdf`. |
| P1 | FANNG | RNG approximation | Occlusion-rule pruning; useful contrast to HNSW/NSG neighbor selection. Ingested as `raw/sources/papers/fanng-2016.pdf`. |
| P1 | DPG | KNNG + RNG approximation | Important step between KGraph and NSG/NSSG. Ingested as `raw/sources/papers/dpg-ann-experiments-2016.pdf`. |
| P1 | HCNNG | MST | Only MST-family representative in the survey; useful because it performs well on hard datasets. |
| P2 | IEH | hashing + KNNG | Less central but useful for seed acquisition and hash/tree comparisons. |

## Deferred Or Not Downloaded In This Pass

| Candidate | Reason |
|---|---|
| SPTAG manual/source | The survey cites SPTAG primarily as a Microsoft library/manual/GitHub reference rather than a standalone PDF paper. Existing vault coverage already includes [SPANN](../entities/spann.md), which depends on SPTAG. |
| Annoy | ANN-Benchmarks evaluates Annoy, but it is primarily a library/GitHub artifact rather than a paper. Keep it as a future entity only if library baselines become a focus. |
| HCNNG | High value as the survey's MST-family representative, but the available source found in this pass was ScienceDirect/Elsevier rather than a stable downloadable PDF. |
| Pruned Bi-directed kNN Graph / PANNG 2016 | High value for NGT, but a stable PDF was not found quickly. The ONNG optimization paper is now ingested as the accessible NGT-family source. |
| IEH | Lower priority; useful mainly for seed acquisition/hashing contrast after KGraph, NGT, EFANNA, FANNG, DPG, FLANN, and FALCONN are ingested. |

## Candidate Queue From SVFusion

| Priority | Candidate | Why it matters |
|---|---|---|
| P0 | CAGRA | Static GPU graph construction/search baseline extended by SVFusion. |
| P0 | FreshDiskANN | Streaming/dynamic graph ANN baseline; complements SPFresh. |
| P1 | GGNN | GPU graph nearest-neighbor search baseline for static GPU-resident settings. |
| P1 | SONG and GANNS | Earlier GPU graph ANN systems cited by SVFusion. |
| P1 | PilotANN | Memory-bounded GPU vector search; directly adjacent to out-of-HBM designs. |
| P1 | Big ANN / NeurIPS Streaming Challenge | Benchmark protocol for streaming vector search and large-scale dynamic workloads. |
| P2 | Quake, HAKES, Manu | Emerging vector DB / adaptive indexing systems for production and cloud-native settings. |

## Synthesis

ANN-Benchmarks tells us how to compare implementations; the graph survey tells us how to attribute graph performance to components; SVFusion shows that modern systems need a third layer covering memory residency, streaming updates, and concurrency.

For future work, the most valuable additions are not isolated new systems but bridge papers:

- **KGraph/NN-Descent:** connects graph construction, survey taxonomy, and ANN-Benchmarks.
- **SPTAG/PANNG:** connects production libraries, seed acquisition, and practical baselines.
- **CAGRA/FreshDiskANN:** connects GPU and streaming-update systems to current hardware-aware ANN work.
- **FLANN/Annoy/FALCONN:** keeps the non-graph benchmark baseline honest.

## Open Questions

- Should candidate queue pages link to PDFs only after raw sources are downloaded, or should external links be allowed in analysis pages?
- Should the vault create lightweight entity pages for KGraph, SPTAG, PANNG, and CAGRA before their primary papers are ingested?
- Which benchmark protocol should become the default for streaming ANN: Big ANN streaming, a custom SPFresh/FreshDiskANN workload, or a new vault-specific matrix?
