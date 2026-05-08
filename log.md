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
