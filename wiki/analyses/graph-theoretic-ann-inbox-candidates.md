---
id: graph-theoretic-ann-inbox-candidates
type: analysis
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - proximity-graph
  - rng
  - literature-map
source_count: 15
sources:
  - raw/sources/papers/graph-based-anns-survey-2021.pdf
  - raw/sources/papers/kleinberg-small-world-2000.pdf
  - raw/sources/papers/navigable-small-world-graph-ann-2014.pdf
  - raw/sources/papers/monotonic-proximity-graphs-2021.pdf
  - raw/sources/papers/satellite-system-graph-2019.pdf
  - raw/sources/papers/relative-nn-descent-2023.pdf
  - raw/sources/papers/rnsg-2026.pdf
  - https://www.sciencedirect.com/science/article/pii/0031320380900667/pdf
  - https://doi.org/10.1109/5.163414
  - https://doi.org/10.1145/116873.116880
  - https://www.sciencedirect.com/science/article/pii/S0306437925000493
  - https://www.sciencedirect.com/science/article/abs/pii/S0167865524003556
  - https://www.sciencedirect.com/science/article/abs/pii/S0306437920300181
  - https://doi.org/10.1137/0211059
  - https://dl.acm.org/doi/10.1145/1007352.1007406
related:
  - approximate-nearest-neighbor-search
  - hnsw
  - nsg
  - diskann
  - vbase
confidence: medium
---

# Graph-Theoretic ANN Inbox Candidates

## Scope

This page records a first pass over graph-theoretic papers worth adding to the vault for the ANN graph-index branch. The goal is to fill the gap between existing system/source notes on [HNSW](../entities/hnsw.md), [NSG](../entities/nsg.md), and [DiskANN](../entities/diskann.md) and the older proximity-graph theory that explains why greedy graph traversal, edge diversity, and RNG/MRNG-style pruning work.

Selection bias: prioritize papers that clarify an idea already appearing in the current vault: proximity graphs, RNG/MRNG/HSP/MSNET, Delaunay/Voronoi navigation, navigable small-world routing, and graph construction/pruning strategies for ANN. Pure computational-geometry papers are lower priority unless they directly explain a design axis used by graph ANN systems.

## Ingested From Inbox

| Source file | Paper | Why it belongs |
|---|---|---|
| `raw/sources/papers/kleinberg-small-world-2000.pdf` | Jon Kleinberg, "The Small-World Phenomenon: An Algorithmic Perspective" | The theoretical source for navigability by greedy local decisions and long-range links; useful context for NSW/HNSW rather than a vector-search paper itself. |
| `raw/sources/papers/navigable-small-world-graph-ann-2014.pdf` | Malkov et al., "Approximate nearest neighbor algorithm based on navigable small world graphs" | Direct predecessor to HNSW; explicitly links greedy graph search, Delaunay approximation, long-range links, and metric-space ANN. |
| `raw/sources/papers/graph-based-anns-survey-2021.pdf` | Wang et al., "A Comprehensive Survey and Experimental Comparison of Graph-Based Approximate Nearest Neighbor Search" | Supplemental survey found in inbox during ingest; useful taxonomy for base graphs, graph ANN components, and RNG-based method comparisons. |
| `raw/sources/papers/monotonic-proximity-graphs-2021.pdf` | Zhu and Zhang, "Understanding and Generalizing Monotonic Proximity Graphs for Approximate Nearest Neighbor Search" | Theoretical treatment of MRNG and generalized monotonic proximity graphs; directly relevant to the NSG/MRNG story. |
| `raw/sources/papers/satellite-system-graph-2019.pdf` | Fu, Wang, and Cai, "High Dimensional Similarity Search with Satellite System Graph" | A graph-index follow-up to NSG that studies MSNET-style monotonicity and unindexed query compatibility. |
| `raw/sources/papers/relative-nn-descent-2023.pdf` | Ono and Matsui, "Relative NN-Descent" | Practical graph construction paper combining NN-Descent with an RNG strategy, useful for understanding construction cost rather than only query traversal. |
| `raw/sources/papers/rnsg-2026.pdf` | Zou et al., "RNSG: A Range-Aware Graph Index for Efficient Range-Filtered Approximate Nearest Neighbor Search" | Modern range-filtered vector-database graph paper built around range-aware relative neighborhood graph (RRNG); relevant to filtered ANN and vector DB topics. |

## High-Priority Candidates Not Yet Downloaded

| Paper | Priority | Reason | Acquisition note |
|---|---:|---|---|
| Toussaint, "The relative neighbourhood graph of a finite planar set" (1980) | 1 | Source paper for RNG; establishes the MST/RNG/Delaunay relationship that later ANN papers reuse conceptually. | ScienceDirect PDF/landing page located, but direct `curl` returned a 403 challenge. |
| Jaromczyk and Toussaint, "Relative Neighborhood Graphs and Their Relatives" (1992) | 1 | Best compact survey of RNG, Gabriel graphs, beta-skeletons, algorithms, and applications. | IEEE DOI and ResearchGate copies located; should fetch from an allowed library/source later. |
| Dearholt et al., "Monotonic Search Networks for Computer Vision Databases" (1988) | 1 | Historical MSNET source cited by NSG/HSP-line work for "search without backtracking." | Bibliographic trail located through later papers; needs PDF/source discovery. |
| Aurenhammer, "Voronoi Diagrams - A Survey of a Fundamental Geometric Data Structure" (1991) | 2 | Delaunay/Voronoi background for exact greedy search and the Delaunay approximation language used in NSW/HNSW. | ACM DOI located; PDF access depends on source availability. |
| Krauthgamer and Lee, "Navigating Nets: Simple Algorithms for Proximity Search" (2004) | 2 | Theoretical proximity search in doubling metrics; useful for separating metric-space navigation theory from graph ANN engineering. | ACM DOI located; author-hosted PDF endpoint was not reachable from this environment. |
| Shimomura et al., "A survey on graph-based methods for similarity searches in metric spaces" (2021) | 2 | Modern survey/taxonomy for graph-based similarity search in metric spaces, including exact/approximate graph search behavior. | ScienceDirect/TU Eindhoven pages located; direct PDF not fetched. |
| Foster, Chavez, and Kimia, "Finding HSP neighbors via an exact, hierarchical approach" (2025) | 2 | HSP/MRNG/RNG bridge with exact monotonic proximity graph computation and explicit discussion of missed long-range links. | Open-access ScienceDirect page located, but direct PDF fetch hit a 403 challenge. |
| "Generalized relative neighborhood graph (GRNG) for similarity search" (2024/2025) | 3 | Modern unification of RNG/HSP/MRNG-style ideas for similarity search; likely useful after the classic RNG/HSP line is ingested. | ScienceDirect abstract located; PDF not fetched. |
| Yao, "On Constructing Minimum Spanning Trees in k-Dimensional Spaces and Related Problems" (1982) | 3 | Source of Yao-graph/cone-neighbor ideas that can help explain angular diversity and geometric-spanner pruning, but less directly tied to current ANN pages. | SIAM DOI located; likely lower priority unless spanner theory becomes a focus. |

## Completed First Ingest Batch

The first ingest batch created a new [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md) topic plus source notes/entities for Kleinberg small-world navigability, NSW, MRNG generalization, SSG/NSSG, Relative NN-Descent, and RNSG. The PDFs were moved out of `raw/inbox/` into `raw/sources/papers/`.

## Suggested Next Ingest Order

1. Find stable copies of Toussaint's 1980 RNG paper and Jaromczyk/Toussaint's 1992 RNG survey, then create RNG/Gabriel/beta-skeleton context.
2. Find the Dearholt MSNET paper, since MRNG/SSG papers lean on monotonic search networks.
3. Add the Aurenhammer Voronoi survey and Navigating Nets only if the topic needs more exact geometry/doubling-metric context.
4. Add HHSP/GRNG after the classic RNG/MSNET path is stable, since they are modern refinements.

## Interpretation For The Vault

The existing vault already has strong coverage of graph ANN systems, but it currently treats "graph quality" mostly as an empirical systems choice. The missing conceptual layer is:

- RNG/proximity graphs explain why edge redundancy can be pruned without destroying local navigability.
- MSNET/MRNG/HSP explain the monotonic path property that NSG-like papers use to reason about greedy traversal.
- Navigable small-world theory explains why hierarchy and long-range links reduce path length beyond local proximity alone.
- Modern construction papers show the practical cost of approximating these ideal graphs at million/billion scale.

## Open Questions

- Should the vault create a standalone topic page for "Proximity Graph Theory for ANN" after the first two proximity papers are ingested?
- Should RNG/HSP/MRNG be separate entity pages, or should they first live as sections under a topic page?
- Which of the older classic papers can be legally and stably fetched as PDFs rather than only linked by DOI?
