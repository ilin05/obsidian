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

## [2026-04-08 18:55] ingest | CXL-Vector project docs and morsel scheduling lineage

- Ingested project-authored sources from `raw/`: `APP_FLOW.md`, `BACKEND_STRUCTURE.md`, `CXL_HNSW_CODE_ARCHITECTURE_ANALYSIS copy.md`, `CXL.md`, `DATA_LAYOUT_PROGRESS.md`, `MORSEL_ARCHITECTURE.md`, `PRD.md`, `TECH_STACK.md`, and `morsel.pdf`.
- Added nine source notes under `wiki/source-notes/` to capture scope, architecture, flow, CXL assumptions, layout, morsel scheduling, implementation stack, thesis-style system analysis, and the original morsel-driven parallelism paper.
- Added two entity pages: `wiki/entities/cxl-vector.md` and `wiki/entities/morsel-driven-parallelism.md`.
- Added one overview page: `wiki/overview/cxl-vector-project-overview.md`.
- Added three project topics: `wiki/topics/cxl-hnsw-runtime-design.md`, `wiki/topics/runtime-layout-and-persistence.md`, and `wiki/topics/morsel-driven-batched-hnsw-search.md`.
- Added one durable synthesis page: `wiki/analyses/cxl-vector-thesis-positioning.md` to preserve paper-facing wording constraints, contribution boundary, and cross-document tensions.
- Updated `wiki/topics/approximate-nearest-neighbor-search.md`, `wiki/topics/disaggregated-memory-vector-search.md`, and `wiki/topics/cxl-disaggregated-memory-systems.md` so the project is integrated into the broader ANN/CXL branch.
- Updated `index.md` so the new overview/topic/entity/source-note/analysis pages are reachable from root navigation.
- Performed a minimal implementation-grounding pass by verifying that the key module directories and cited test paths under `/Users/qinjianwu/CXL-Vector` exist.
- Working synthesis: the project is best framed as a CXL-aware, read-only HNSW serving runtime whose thesis core is runtime layout conversion plus batched coarse-to-exact execution under remote-memory constraints.
- Open questions: align wording between conservative scope documents and stronger internal analysis, and resolve exactly how upper-layer routing should be described across single-query and batch paths.

## [2026-04-10 00:00] refactor | Tidy raw/ directory structure

- Reorganized all 26 flat files from `raw/` root into three logical subdirectories under `raw/sources/`.
- Moved 16 academic PDFs to `raw/sources/papers/` with slug names matching wiki source-notes (e.g., `ansmet-2025.pdf`, `hnsw-2016.pdf`).
- Moved 8 CXL-Vector internal project MDs to `raw/sources/project/` with snake-case names (e.g., `app-flow.md`, `prd.md`).
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

**Working synthesis:** The vault now has a complete related-work baseline coverage for the CXL-Vector paper. Key relationship: FAISS (in-DRAM library baseline) → ScaNN (best in-DRAM quantized ANN) → SPANN (best DRAM+SSD system, deployed at Bing scale) → CXL-Vector (DRAM+CXL, graph-based, finer-grained access). The CXL memory characterization paper (Micro 2023) provides the empirical hardware grounding: true CXL at 29–41ns justifies HNSW graph traversal on CXL (unlike SSD where only sequential IVF posting lists are feasible).

**Open questions:** (1) At what scale/hardware budget does CXL-Vector outperform SPANN? (2) Can score-aware quantization improve CXL-Vector's SQ8 coarse routing? (3) What billion-scale benchmark protocol should CXL-Vector use given ANN-Benchmarks is in-memory only?

## [2026-04-10 02:00] update | Hardware configuration — confirmed NUMA topology and ISA

Updated wiki with confirmed hardware specification from lscpu output. Key facts:

- **CPU**: 2× Intel Xeon Gold 5520+ (Emerald Rapids, 5th Gen Xeon, Family 6 Model 207 Stepping 2)
- **Cores/threads**: 2×28 cores, 112 logical threads (SMT enabled); experiments use 56 threads on node1
- **NUMA**: node1 = socket 1 CPUs (28–55) + DRAM + experiment threads; node3 = CXL device (memory-only, no CPUs)
- **ISA**: AVX-512F/BW/VL/VNNI/VBMI2, AVX-VNNI, AMX-INT8/BF16 — `level0_use_vnni=true` is hardware-native
- **Cache**: L2 = 2 MiB/core (112 MiB total); L3 = ~52.5 MiB/socket (105 MiB total)
- **CXL placement**: `vec_segment_` 488 MB VMA bound to node3 via `mmap + mbind`; DRAM segments on node1

**Pages updated:**
- `wiki/source-notes/cxl-vector-tech-stack-2026.md`: new "Confirmed Hardware Platform" section with full CPU/ISA/cache/NUMA tables
- `wiki/entities/cxl-vector.md`: new "Hardware Platform" section with condensed spec and NUMA role description
- `wiki/source-notes/cxl-hnsw-code-architecture-analysis-2026.md`: new "Hardware Configuration" table added; source path corrected to `raw/sources/project/`
- Source paths in entity frontmatter corrected to reflect reorganized `raw/sources/project/` structure

## [2026-05-08 11:20] refactor | Refresh CXL-Vector wiki from latest paper docs

- Rebuilt the CXL-Vector knowledge cluster from the latest root-level documents: `vldb-paper-outline.md`, `vldb-paper-intro-draft.md`, and `cross-query-rerank-coalescing-plan.md`.
- Deleted stale project-topic pages that represented the older CXL-Vector ingestion: `wiki/topics/morsel-driven-batched-hnsw-search.md` and `wiki/topics/runtime-layout-and-persistence.md`.
- Added refreshed durable pages:
  - `wiki/overview/cxl-vector-vldb-paper.md`
  - `wiki/entities/cxl-vector.md`
  - `wiki/topics/cxl-vector-commodity-cxl-hnsw-serving.md`
  - `wiki/topics/cross-query-rerank-coalescing.md`
  - `wiki/analyses/cxl-vector-vldb-positioning.md`
- Added source notes for the latest working docs:
  - `wiki/source-notes/cxl-vector-vldb-outline-2026.md`
  - `wiki/source-notes/cxl-vector-introduction-draft-2026.md`
  - `wiki/source-notes/cross-query-rerank-coalescing-plan-2026.md`
- Updated `index.md` and the broader ANN/CXL topic pages so navigation points to the refreshed paper-driven CXL-Vector structure.
- Current synthesis: CXL-Vector should now be treated as a commodity-CXL VLDB paper project whose strongest framing is "PCIe bandwidth as a first-class scheduling resource for HNSW serving." Cross-query rerank coalescing is a seed-stage mechanism pending overlap-rate measurement.
