# Log

This file is the append-only chronological history of wiki operations.

## [2026-04-08 00:00] refactor | Initial scaffold

- Created the initial vault scaffold for the LLM wiki.
- Added `AGENTS.md`, `index.md`, and `log.md`.
- Created the base directory structure under `raw/`, `wiki/`, and `templates/`.
- Next step: add the first real source or create the first durable wiki page.

## [2026-04-08 11:12] ingest | QJL, Product Quantization, TurboQuant

- Ingested `raw/QJL.pdf`, `raw/pq.pdf`, and `raw/TurboQuant.pdf`.
- Added source notes for QJL, Product Quantization, and TurboQuant under `wiki/source-notes/`.
- Added synthesis pages under `wiki/overview/`, `wiki/topics/`, and `wiki/entities/` to connect retrieval-oriented quantization with KV-cache compression.
- Updated `index.md` so the new pages are navigable from the root catalog.
- Working synthesis: Product Quantization is the foundational retrieval baseline, QJL is the specialized zero-overhead KV-cache method, and TurboQuant is the broader online framework that reuses QJL on residuals.
- Open questions: whether TurboQuant beats plain QJL head-to-head on KV-cache tasks at matched latency budgets, and how much tuned offline PQ still outperforms online methods on mature ANN workloads.

## [2026-04-08 11:18] ingest | RaBitQ extension for ANN search

- Ingested `raw/rabbitq.pdf`.
- Added a source note and entity page for the multi-bit RaBitQ extension under `wiki/source-notes/` and `wiki/entities/`.
- Updated the overview and topic synthesis pages so the retrieval branch now distinguishes PQ-style learned codebooks from RaBitQ-style randomized data-oblivious quantization.
- Updated `index.md` so the new method and source are navigable from the root catalog.
- Working synthesis: RaBitQ strengthens the retrieval side of the vault as a theory-heavy alternative to PQ, while TurboQuant remains the broader framework trying to span both retrieval and inference.
- Open questions: whether RaBitQ or TurboQuant is the stronger practical retrieval path under equal engineering effort, and when PQ still wins enough on mature ANN workloads to justify offline indexing complexity.

## [2026-04-08 17:18] ingest | HNSW, NSG, DiskANN, CXL-ANNS, second-tier-memory trade-offs, d-HNSW

- Ingested six new ANN/system sources: `raw/HNSW.pdf`, `raw/NSG.pdf`, `raw/DiskANN.pdf`, `raw/CXL-ANNS.pdf`, `raw/Characterizing the Dilemma of Performance and Index Size in Billion-Scale Vector Search .pdf`, and `raw/Efficient Vector Search on Disaggregated Memory with d-HNSW.pdf`.
- Added six source notes under `wiki/source-notes/` for the above papers.
- Added five new entity pages under `wiki/entities/`: HNSW, NSG, DiskANN, CXL-ANNS, and d-HNSW.
- Added two new systems topics under `wiki/topics/`: `second-tier-memory-for-vector-search` and `disaggregated-memory-vector-search`.
- Rewrote `wiki/topics/approximate-nearest-neighbor-search.md` to integrate the quantization branch with the graph/system co-design branch.
- Updated `index.md` so all newly added topic/entity/source-note pages are navigable from the root catalog.
- Working synthesis: the vault now distinguishes ANN method design (PQ/RaBitQ/TurboQuant, HNSW/NSG) from ANN systems design (DiskANN on SSD, CXL/RDMA/second-tier-memory pathways), making the memory-tier trade-off explicit.
- Open questions: crossover point between SSD-centric and disaggregated-memory-centric serving under matched hardware budgets, and how robust published wins are under mixed update/query production workloads.

## [2026-04-08 17:20] lint | ANN systems branch cross-link health check

- Checked wiki markdown links for broken references across `wiki/` and `index.md`; no broken local links found.
- Checked inbound-link counts and found weak linkage on `wiki/source-notes/performance-index-size-dilemma-2024.md`.
- Added explicit cross-links from `wiki/topics/second-tier-memory-for-vector-search.md` and `wiki/topics/disaggregated-memory-vector-search.md` to the performance/index-size source note.
- Expanded related links in `wiki/source-notes/performance-index-size-dilemma-2024.md` to include disaggregated-memory pathways (`cxl-anns`, `d-hnsw`).
- Remaining risk: no structural contradictions detected at markdown-link level, but benchmark comparability across papers still needs manual methodological alignment when building seminar-level comparative claims.

## [2026-04-08 17:36] ingest | CXLfork, PolarCXLMem, ANSMET and CXL cross-domain systems

- Ingested four new raw sources: `raw/CXLfork.pdf`, `raw/Unlocking the Potential of CXL for Disaggregated Memory in Cloud-Native Databases.md`, `raw/Unlocking the Potential of CXL for Disaggregated Memory in CloudNative Databases.pdf`, and `raw/ANSMET- .pdf`.
- Added three source notes under `wiki/source-notes/`: `cxlfork-2025.md`, `polarcxlmem-2025.md`, and `ansmet-2025.md`.
- Added three entity pages under `wiki/entities/`: `cxlfork.md`, `polarcxlmem.md`, and `ansmet.md`.
- Added one new topic page: `wiki/topics/cxl-disaggregated-memory-systems.md` to connect CXL work across ANN, cloud databases, and serverless runtimes.
- Updated `wiki/topics/approximate-nearest-neighbor-search.md` to include ANSMET as a near-memory acceleration branch.
- Updated `wiki/topics/disaggregated-memory-vector-search.md` and `index.md` for navigation and cross-link coverage.
- Working synthesis: the vault now separates memory-fabric-aware ANN serving (CXL/RDMA) from broader CXL-native system-interface redesigns (process cloning, database buffer/recovery/coherence).
- Open questions: portability of current gains across future CXL switch generations, and which abstractions can be standardized across ANN, DBMS, and serverless stacks.

## [2026-04-08 17:42] ingest | Patience in Proximity (HNSW early termination)

- Ingested `raw/patience.pdf`.
- Added `wiki/source-notes/patience-in-proximity-2025.md` with source-grounded summary of the saturation-based early-termination strategy for HNSW traversal.
- Added `wiki/entities/patience-in-proximity.md` and linked it with `wiki/entities/hnsw.md`.
- Updated `wiki/topics/approximate-nearest-neighbor-search.md` to include the HNSW-traversal-heuristic branch.
- Updated `index.md` so the new entity/source note are reachable from root navigation.
- Working synthesis: this source strengthens the "lightweight runtime heuristics on top of HNSW" lane, complementary to system-level accelerations like ANSMET and disaggregated-memory serving.
- Open questions: how robust patience thresholds are across datasets, and whether this heuristic composes cleanly with DiskANN/HNSW production variants under strict recall targets.

## [2026-04-10 00:00] refactor | Tidy raw/ directory structure

- Reorganized all 26 flat files from `raw/` root into three logical subdirectories under `raw/sources/`.
- Moved 16 academic PDFs to `raw/sources/papers/` with slug names matching wiki source-notes (e.g., `ansmet-2025.pdf`, `hnsw-2016.pdf`).
- Moved 2 hand-written notes/summaries to `raw/sources/notes/` (`turboquant-notes.md`, `polarcxlmem-notes.md`).
- Fixed malformed filenames: removed trailing space/dash from `ANSMET- .pdf`, renamed all-caps MDs to snake-case, corrected Chinese filename encoding for TurboQuant CN summary.
- `raw/inbox/`, `raw/sources/`, and `raw/assets/` are now empty of misplaced files; all content is properly nested.
- No wiki pages were changed; this was a raw/ housekeeping pass only.

## [2026-04-10 01:00] ingest | FAISS, ScaNN, SPANN, Milvus, ANN-Benchmarks, CXL Memory Characterization

Ingested 7 new sources from `raw/inbox/` (2 FAISS papers counted as one entity):

**Papers ingested:**
- `faiss.pdf` → `raw/sources/papers/faiss-gpu-2017.pdf` (Johnson et al. 2017, GPU IVFPQ + WarpSelect)
- `faiss_2024.pdf` → `raw/sources/papers/faiss-library-2025.pdf` (Douze et al. 2025, Faiss library overview)
- `ScaNN.pdf` → `raw/sources/papers/scann-2020.pdf` (Guo et al. ICML 2020, anisotropic quantization)
- `SPANN.pdf` → `raw/sources/papers/spann-2021.pdf` (Chen et al. NeurIPS 2021, memory-disk IVF)
- `Milvus.pdf` → `raw/sources/papers/milvus-2021.pdf` (Wang et al. SIGMOD 2021, vector DBMS)
- `ANN-Benchmarks.pdf` → `raw/sources/papers/ann-benchmarks-2018.pdf` (Aumüller et al. 2018)
- `CXL-Memory.pdf` → `raw/sources/papers/cxl-memory-characterization-2023.pdf` (Sun et al. Micro 2023)

**New source notes created (7):** `faiss-gpu-2017.md`, `faiss-library-2025.md`, `scann-2020.md`, `spann-2021.md`, `milvus-2021.md`, `ann-benchmarks-2018.md`, `cxl-memory-characterization-2023.md`

**New entity pages created (5):** `faiss.md`, `scann.md`, `spann.md`, `milvus.md`, `ann-benchmarks.md`

**Topic pages updated (3):**
- `approximate-nearest-neighbor-search.md`: expanded to three-layer view (compression, graph, systems); added FAISS, ScaNN, SPANN, Milvus, ANN-Benchmarks; updated system progression narrative.
- `second-tier-memory-for-vector-search.md`: added SPANN as primary DRAM+SSD baseline; added CXL memory characterization for hardware grounding; added tier comparison table.
- `cxl-disaggregated-memory-systems.md`: added CXL memory characterization as hardware grounding section; added CXL latency/bandwidth findings table.

**index.md updated:** 5 new entity entries, 7 new source note entries added to navigation.

**Working synthesis:** The vault now has complete baseline coverage for the ANN systems branch. Key relationship: FAISS (in-DRAM library baseline) → ScaNN (in-DRAM quantized ANN) → SPANN (DRAM+SSD system, deployed at Bing scale) → CXL/RDMA disaggregated-memory systems.

**Open questions:** (1) At what scale/hardware budget do CXL/RDMA memory systems outperform SPANN? (2) Can score-aware quantization improve compressed coarse routing? (3) What billion-scale benchmark protocol should be used given ANN-Benchmarks is in-memory only?

## [2026-05-08 11:42] ingest | GustANN, FusionANNS, and billion-scale vector search literature map

Processed `raw/sources/papers/gustann-2025.pdf` and `raw/sources/papers/fusionanns-2025.pdf` into source notes and entity pages. Updated the ANN and second-tier-memory topic pages to account for SSD+GPU systems as adjacent related work. Created `wiki/analyses/billion-scale-vector-search-literature-map.md` with external paper leads not yet ingested, including RUMMY, SmartANNS, Starling, BANG, SPFresh, VBASE, GRIP, iDEC, ParANN, and iQAN.

Touched pages: `wiki/source-notes/gustann-2025.md`, `wiki/source-notes/fusionanns-2025.md`, `wiki/entities/gustann.md`, `wiki/entities/fusionanns.md`, `wiki/topics/approximate-nearest-neighbor-search.md`, `wiki/topics/second-tier-memory-for-vector-search.md`, `wiki/analyses/billion-scale-vector-search-literature-map.md`, `index.md`.

Unresolved questions: decide which SSD+GPU papers should be fully ingested next, and whether the literature map should include a taxonomy figure separating DRAM-only, SSD-only, SSD+GPU, SmartSSD/NDP, CXL, and RDMA-disaggregated systems.

## [2026-05-08 11:52] refactor | Remove stale project cluster and broken local references

Removed stale project-specific CXL ANN pages and root drafts from the maintained vault graph. Updated ANN, CXL, SSD/GPU, benchmark, and source-note pages to avoid links to deleted project pages. Normalized stale raw source paths to current `raw/sources/` locations and removed missing TurboQuant image links that created broken Obsidian graph nodes.

## [2026-05-08 12:12] ingest | RUMMY, SmartANNS, VBASE, Starling, BANG, and SPFresh

Downloaded and ingested six relevant billion-scale vector search papers not previously present in the repo: `raw/sources/papers/rummy-2024.pdf`, `raw/sources/papers/smartanns-2024.pdf`, `raw/sources/papers/vbase-2023.pdf`, `raw/sources/papers/starling-2024.pdf`, `raw/sources/papers/bang-2024.pdf`, and `raw/sources/papers/spfresh-2023.pdf`.

Created source notes: `wiki/source-notes/rummy-2024.md`, `wiki/source-notes/smartanns-2024.md`, `wiki/source-notes/vbase-2023.md`, `wiki/source-notes/starling-2024.md`, `wiki/source-notes/bang-2024.md`, and `wiki/source-notes/spfresh-2023.md`.

Created entity pages: `wiki/entities/rummy.md`, `wiki/entities/smartanns.md`, `wiki/entities/vbase.md`, `wiki/entities/starling.md`, `wiki/entities/bang.md`, and `wiki/entities/spfresh.md`.

Updated synthesis pages: `wiki/topics/approximate-nearest-neighbor-search.md`, `wiki/topics/second-tier-memory-for-vector-search.md`, and `wiki/analyses/billion-scale-vector-search-literature-map.md`. Updated `index.md` for navigation.

Working synthesis: the billion-scale vector search branch now covers GPU/host-memory execution, SmartSSD/NDP, vector-relational query semantics, segment-level disk graph layout, single-GPU graph search, and fresh-update maintenance in addition to the existing DiskANN/SPANN/GustANN/FusionANNS/CXL/RDMA coverage.

Unresolved questions: remaining candidate papers for future ingest are GRIP, iDEC, ParANN/ParlayANN, and iQAN; decide whether these should be source-note depth or literature-map-only references.

## [2026-05-08 12:21] refactor | Move inbox papers to organized sources

Moved the remaining inbox PDFs into `raw/sources/papers/`: `raw/sources/papers/gustann-2025.pdf` and `raw/sources/papers/fusionanns-2025.pdf`.

Updated source frontmatter references in GustANN/FusionANNS source notes, entity pages, topic pages, and the billion-scale literature map so they no longer point to `raw/inbox/`.

`raw/inbox/` now only retains `.gitkeep`.

## [2026-05-08 14:55] query | Graph-theoretic ANN inbox candidates

Explored graph-theoretic papers related to the current ANN graph-index branch, especially proximity graphs, RNG/MRNG/HSP/MSNET, and navigable small-world ideas connected to HNSW, NSG, and DiskANN. Added six PDFs to `raw/inbox/`: Kleinberg small-world navigability, Malkov et al. NSW, Zhu/Zhang MRNG generalization, Satellite System Graph, Relative NN-Descent, and RNSG/RRNG. Created `wiki/analyses/graph-theoretic-ann-inbox-candidates.md` with the selection rationale, priority order, and high-value candidates not yet downloaded. Updated `index.md`.

Unresolved questions: locate stable downloadable copies for Toussaint 1980 RNG, Jaromczyk/Toussaint 1992 RNG survey, Dearholt MSNET, Aurenhammer Voronoi survey, Navigating Nets, HHSP, and GRNG; decide whether to create a standalone topic page for proximity graph theory after ingest.

## [2026-05-08 15:00] ingest | Proximity graph theory for ANN

Ingested the graph-theoretic ANN PDFs from `raw/inbox/` and moved them to `raw/sources/papers/`: `kleinberg-small-world-2000.pdf`, `navigable-small-world-graph-ann-2014.pdf`, `monotonic-proximity-graphs-2021.pdf`, `satellite-system-graph-2019.pdf`, `relative-nn-descent-2023.pdf`, `rnsg-2026.pdf`, and the supplemental `graph-based-anns-survey-2021.pdf`.

Created source notes for all seven papers, a new topic page `wiki/topics/proximity-graph-theory-for-ann.md`, and five entity pages: `monotonic-relative-neighborhood-graph.md`, `navigable-small-world-graph.md`, `satellite-system-graph.md`, `relative-nn-descent.md`, and `rnsg.md`. Updated `index.md`, `wiki/topics/approximate-nearest-neighbor-search.md`, HNSW/NSG/DiskANN/VBASE entity pages, and relevant HNSW/NSG/DiskANN source notes. Updated `wiki/analyses/graph-theoretic-ann-inbox-candidates.md` so the first batch is recorded as ingested, not still pending in inbox.

Unresolved questions: still need stable copies for Toussaint RNG, Jaromczyk/Toussaint RNG survey, Dearholt MSNET, Aurenhammer Voronoi, Navigating Nets, HHSP, and GRNG; RNSG publication metadata remains uncertain because the arXiv PDF has placeholder PVLDB/DOI fields.

## [2026-05-08 15:16] ingest | ANN benchmarks, graph survey expansion, and SVFusion

Ingested the new inbox contents. `raw/inbox/sv-fusion.pdf` was moved to `raw/sources/papers/svfusion-2026.pdf` and converted into `wiki/source-notes/svfusion-2026.md` plus `wiki/entities/svfusion.md`. `raw/inbox/ann-benchmarks.pdf` was verified as byte-identical to the existing canonical `raw/sources/papers/ann-benchmarks-2018.pdf`; the duplicate inbox copy was removed after updating the canonical ANN-Benchmarks note and entity.

Expanded `wiki/source-notes/ann-benchmarks-2018.md` with implementation-vs-algorithm benchmarking lessons, evaluated algorithm families, key results, and connections to graph/system benchmarks. Expanded `wiki/source-notes/graph-based-anns-survey-2021.md` with the four-family graph taxonomy, 13-algorithm map, seven pipeline components, empirical lessons, and missing primary-source coverage.

Created `wiki/topics/ann-benchmarking-methodology.md` to separate implementation benchmarking, graph-component attribution, and hardware/update-aware system benchmarking. Created `wiki/analyses/ann-benchmarking-and-graph-coverage-gaps.md` as a candidate queue seeded by ANN-Benchmarks, Graph-Based ANNS Survey 2021, and SVFusion references.

Updated `wiki/topics/approximate-nearest-neighbor-search.md`, `wiki/topics/second-tier-memory-for-vector-search.md`, `wiki/analyses/billion-scale-vector-search-literature-map.md`, `wiki/entities/fusionanns.md`, `wiki/entities/bang.md`, `wiki/entities/spfresh.md`, and `index.md`.

Working synthesis: the ANN branch now has a clearer evaluation stack: ANN-Benchmarks for in-memory implementation-level Pareto frontiers, Graph-Based ANNS Survey 2021 for graph-family and component attribution, and SVFusion/SPFresh-style metrics for streaming updates and heterogeneous memory tiers.

Unresolved questions: prioritize primary-source ingests for KGraph/NN-Descent, SPTAG, NGT, CAGRA, FreshDiskANN, and the Big ANN streaming benchmark protocol; decide whether lightweight entity pages should exist before those primary sources are downloaded.

## [2026-05-08 15:29] query | ANN benchmark and graph survey inbox candidates

Explored candidate papers based on `wiki/entities/ann-benchmarks.md` and `raw/sources/papers/graph-based-anns-survey-2021.pdf`. Downloaded eight valuable, non-duplicate PDFs into `raw/inbox/`: `nn-descent-2011.pdf`, `gnns-knn-graph-2011.pdf`, `efanna-2016.pdf`, `fanng-2016.pdf`, `dpg-ann-experiments-2016.pdf`, `ngt-onng-2018.pdf`, `flann-2014.pdf`, and `falconn-lsh-angular-2015.pdf`.

Updated `wiki/analyses/ann-benchmarking-and-graph-coverage-gaps.md` with the downloaded inbox batch, rationale, and deferred candidates. Updated `index.md` to clarify that this analysis now records an inbox candidate queue.

Working synthesis: the next ANN ingest batch should fill the gap between benchmark baselines and graph algorithm internals: KNNG construction (NN-Descent/KGraph), early graph traversal (GNNS), tree-initialized graph construction (EFANNA), RNG-style pruning (FANNG/DPG/NGT), and non-graph benchmark baselines (FLANN/FALCONN).

Unresolved questions: still need stable PDFs or source decisions for SPTAG, HCNNG, PANNG/pruned bi-directed KNNG, Annoy, PyNNDescent, and IEH.

## [2026-05-08 15:43] ingest | ANN graph baseline and non-graph benchmark papers

Ingested the eight new inbox papers selected from ANN-Benchmarks and Graph-Based ANNS Survey 2021, then moved them from `raw/inbox/` into `raw/sources/papers/`: `nn-descent-2011.pdf`, `gnns-knn-graph-2011.pdf`, `efanna-2016.pdf`, `fanng-2016.pdf`, `dpg-ann-experiments-2016.pdf`, `ngt-onng-2018.pdf`, `flann-2014.pdf`, and `falconn-lsh-angular-2015.pdf`.

Created source notes: `wiki/source-notes/nn-descent-2011.md`, `wiki/source-notes/gnns-knn-graph-2011.md`, `wiki/source-notes/efanna-2016.md`, `wiki/source-notes/fanng-2016.md`, `wiki/source-notes/dpg-ann-experiments-2016.md`, `wiki/source-notes/ngt-onng-2018.md`, `wiki/source-notes/flann-2014.md`, and `wiki/source-notes/falconn-lsh-angular-2015.md`.

Created entity pages: `wiki/entities/nn-descent.md`, `wiki/entities/gnns.md`, `wiki/entities/efanna.md`, `wiki/entities/fanng.md`, `wiki/entities/diversified-proximity-graph.md`, `wiki/entities/ngt-onng.md`, `wiki/entities/flann.md`, and `wiki/entities/falconn.md`.

Updated synthesis and navigation pages: `wiki/topics/approximate-nearest-neighbor-search.md`, `wiki/topics/proximity-graph-theory-for-ann.md`, `wiki/topics/ann-benchmarking-methodology.md`, `wiki/source-notes/ann-benchmarks-2018.md`, `wiki/source-notes/graph-based-anns-survey-2021.md`, `wiki/entities/ann-benchmarks.md`, `wiki/entities/nsg.md`, `wiki/source-notes/nsg-2019.md`, `wiki/entities/relative-nn-descent.md`, `wiki/source-notes/relative-nn-descent-2023.md`, `wiki/analyses/ann-benchmarking-and-graph-coverage-gaps.md`, and `index.md`.

Working synthesis: the ANN branch now covers the missing bridge between benchmark baselines and graph-index internals: NN-Descent/KGraph for KNNG construction, GNNS for early graph traversal, EFANNA for tree-initialized graph construction and seeding, FANNG/DPG/NGT/ONNG for practical pruning and degree/path control, and FLANN/FALCONN for non-graph tree/LSH baselines.

Unresolved questions: still need stable primary sources or source decisions for SPTAG, HCNNG, PANNG/pruned bi-directed KNNG, Annoy, PyNNDescent, IEH, CAGRA, FreshDiskANN, and Big ANN streaming benchmark protocols.

## [2026-05-08 15:49] refactor | CXL-Vector paper agent cache and review/writer guides

Created three root-level operating guides for CXL-Vector paper work: `CACHE.md`, `reviewer.md`, and `writer.md`.

`CACHE.md` provides a short read path into the CXL/ANN knowledge base and fixes the intended CXL-Vector framing: capacity-wall motivation, bandwidth-sensitive transparent all-CXL placement, host-local compact traversal state, CXL-resident raw vectors, bounded exact rerank, and matched-recall evaluation.

`reviewer.md` adapts the `vldb-section-reviewer` skill to this vault, including section-specific read paths, CXL-Vector-specific review checks, output file expectations, and minimum-fix bias.

`writer.md` adapts the conservative `vldb-review-writer` workflow for CXL-Vector sections, including claim boundaries, allowed edit actions, forbidden overclaims, and expected writer change-summary output.

Updated `index.md` with an operating-guides section so new agents can find these files before reading deeper wiki pages.

Unresolved questions: once the actual CXL-Vector paper repository path and section filenames are known, these guides may need one line pointing to the paper-local implementation context file.

## [2026-05-08 15:53] refactor | General ANNS system paper cache and review/writer guides

Created root-level general ANNS operating guides: `CACHE.md`, `reviewer.md`, and `writer.md`.

`CACHE.md` is now a compact read path for ANNS system agents, covering the main ANN synthesis, benchmarking methodology, graph survey, proximity-graph theory, in-memory graph baselines, quantization/library baselines, SSD/second-tier systems, GPU/heterogeneous systems, CXL/RDMA/NDP systems, and vector DB semantics.

`reviewer.md` adapts the `vldb-section-reviewer` skill to general ANNS system paper review. It defines claim-specific source paths, baseline expectations, evaluation checks, minimum-fix stance, and exact review/fix output requirements.

`writer.md` provides a conservative writer workflow for ANNS paper sections after review: weaken unsupported claims, clarify mechanisms, preserve matched-recall framing, and avoid invented numbers, baselines, experiments, or citations.

Updated `index.md` operating-guide labels from CXL-Vector-specific to general ANNS-system-specific.

Unresolved questions: if a specific ANNS paper repository has local implementation context, add a short pointer from `CACHE.md` to that paper-local file.

## [2026-05-09 12:36] refactor | ANNS research figure drawing guide and skill

Created a figure-drawing entry point for ANNS system paper work.

Added `FIGURE_CACHE.md` as the short read path for drawing and reviewing ANNS figures. It maps common needs to concrete rendered reference pages: system architecture, data placement, query workflow, graph/algorithm intuition, recall-QPS curves, ablation/sensitivity, and hardware characterization.

Created `wiki/analyses/anns-research-figure-design-guide.md` with a full taxonomy of ANNS paper figures. It separates system figures from experiment figures, summarizes visual patterns across HNSW/NSG/PQ/FAISS, DiskANN/SPANN/Starling/SPFresh, BANG/RUMMY/GustANN/FusionANNS/SVFusion/SmartANNS, CXL-ANNS/CXL memory characterization, ANN-Benchmarks, and Graph-Based ANNS Survey.

Rendered 25 representative PDF pages into `wiki/assets/figure-guide/` for quick visual reference. These include architecture/workflow pages, graph and quantization intuition pages, recall-QPS/Pareto pages, ablation/sensitivity pages, and hardware characterization pages.

Created a vault-local skill at `skills/anns-research-figure-drawing/SKILL.md` for future agents drawing or reviewing ANNS research figures.

Updated `CACHE.md` and `index.md` so the new figure cache, figure guide, and skill are discoverable.

Unresolved questions: future work should add cropped single-figure assets and paper-local figure templates once a concrete target paper repository is available.
