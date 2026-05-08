# ANNS System Reviewer Guide

Use this with the `vldb-section-reviewer` skill for any ANNS, vector-search, or vector-database systems paper section.

## Read Order

1. [CACHE.md](CACHE.md)
2. Target section
3. Implementation/design context, if provided
4. Only the vault pages needed for the section's claims

## Claim-Specific Sources

- General ANNS positioning: `wiki/topics/approximate-nearest-neighbor-search.md`
- Evaluation rules: `wiki/topics/ann-benchmarking-methodology.md`
- Graph taxonomy: `wiki/source-notes/graph-based-anns-survey-2021.md`
- Graph theory/pruning: `wiki/topics/proximity-graph-theory-for-ann.md`
- In-memory graph baselines: HNSW, NSG, NN-Descent, EFANNA, FANNG, DPG, NGT/ONNG
- Quantization/library baselines: FAISS, ScaNN, Product Quantization, RaBitQ, FLANN, FALCONN
- SSD/second-tier systems: DiskANN, SPANN, Starling, SPFresh
- GPU/heterogeneous systems: BANG, RUMMY, GustANN, FusionANNS, SVFusion
- CXL/RDMA/NDP systems: CXL-ANNS, d-HNSW, SmartANNS, CXL memory characterization
- Vector DB / filtered ANN: Milvus, VBASE, RNSG

## Review Stance

Be strict, technical, and conservative. Identify the smallest fix that makes the section credible for VLDB/SIGMOD/PVLDB.

Penalize heavily:

- unclear novelty over HNSW/NSG/DiskANN/SPANN/FAISS/ScaNN or the closest tier-specific system;
- unsupported speedup, throughput, latency, scalability, or low-overhead claims;
- missing recall definition, distance metric, workload, dataset, or baseline configuration;
- missing build time, index size, memory footprint, update cost, or parameter settings;
- vague graph mechanisms: no construction, seeding, pruning, routing, connectivity, or termination rule;
- vague system mechanisms: no placement policy, scheduling rule, cache policy, data movement, or concurrency model;
- missing impact on recall/correctness/approximation quality;
- missing ablations for the claimed core design choices;
- mismatch between section text and implementation context.

## Minimum Baseline Expectations

- In-memory graph claim: compare against HNSW/NSG or justify why not.
- SSD/second-tier claim: compare against DiskANN/SPANN-style systems.
- GPU claim: compare against relevant GPU or host-memory systems such as FAISS GPU, BANG, RUMMY, or CAGRA-like baselines when available.
- CXL/RDMA/NDP claim: compare against CXL-ANNS/d-HNSW/SmartANNS-style ideas plus local-DRAM and transparent placement baselines.
- Vector DB / filtered claim: compare against VBASE/Milvus/filter-aware graph work where relevant.
- Quantization claim: separate compression ratio, recall loss, rerank/refinement, and distance-estimation cost.

## Evaluation Checks

Require matched-recall plots or tables. Prefer p50/p95/p99 latency plus QPS when the system serves online queries. Require build time and memory/index size for graph methods. For dynamic systems, require insert/delete throughput, recall drift, and repair/rebuild cost. For hardware-tier papers, require data movement and capacity accounting.

## Output

Write:

- `reviews/<section-name>-review.md`
- `reviews/<section-name>-fix.md`

Use the exact `vldb-section-reviewer` structure: score `1/10` to `10/10`, readiness exactly `READY: YES`, `READY: ALMOST`, or `READY: NO`, ranked weaknesses, minimum fixes, and exact edit instructions.

Do not invent experiments, numbers, baselines, citations, or results.
