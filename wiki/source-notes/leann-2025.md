---
id: leann-2025
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - graph-index
  - embedding-recomputation
  - storage-efficient
source_count: 1
sources:
  - raw/sources/papers/leann-2025.pdf
related:
  - vector-compression
  - approximate-nearest-neighbor-search
  - hnsw
confidence: medium
---

# LEANN: A Low-Storage Overhead Vector Index

Yichuan Wang, Zhifei Li, Shu Liu, Yongji Wu, Ziming Mao, Yilong Zhao, Xiao Yan, Zhiying Xu, Yang Zhou, Ion Stoica, Sewon Min, Matei Zaharia, and Joseph E. Gonzalez, under review, 2025 (UC Berkeley).

## Summary

LEANN tackles the storage overhead of vector indexes by combining two radical ideas: (1) **on-the-fly embedding recomputation** — instead of storing embedding vectors, re-encode the original data items (e.g., text chunks) at query time using the same embedding model; (2) **high-degree preserving graph pruning** — aggressively prune low-degree nodes' edges while preserving hub-node connectivity. A two-level search uses cheap PQ-based approximate distances to prune candidates before expensive exact distance computation on recomputed embeddings. For RAG workloads where LLM generation dominates end-to-end latency, LEANN trades a small amount of search time for massive storage savings: index size reduced by up to 50× while maintaining downstream accuracy.

## Key Contributions

- **First systematic study of storage as the primary bottleneck** in vector search deployment, especially for personal devices and large-scale datasets.
- **Embedding recomputation**: Observes that graph-based ANNS visits only a small subset of nodes per query — recomputing embeddings on-the-fly for visited nodes is feasible and eliminates the need to store them.
- **Two-level search**: Level 1 uses a lightweight PQ table for approximate distance pruning; Level 2 recomputes exact embeddings for surviving candidates using GPU-batched encoder inference.
- **High-degree preserving graph pruning**: Removes edges from low-degree nodes while preserving hub-node connectivity, significantly reducing index metadata size with minimal accuracy loss.
- **Storage-constrained index building**: Sharded merging pipeline ensures storage never exceeds a small budget even during construction.
- **Update support**: Adding new data does not require full index rebuild.

## Algorithm Details

### Two-Level Search
1. Coarse level: PQ-compressed approximate distances filter candidates using the pruned HNSW graph.
2. Fine level: For top candidates, recompute embeddings via the original encoder (batched on GPU), compute exact distances, and re-rank.

### Graph Pruning
- Identify high-degree hub nodes (visited most frequently during search).
- For low-degree nodes, prune edges that are redundant (other paths exist).
- Preserve all edges of hub nodes to maintain search navigability.

### Index Building
- Sharded merging: process data in shards, build partial HNSW graphs, merge without materializing all embeddings simultaneously.

## Reported Results

- Storage: down to 5% of original data size (~4 GB for a 76 GB text corpus)
- Index size: up to 50× smaller than conventional HNSW+PQ
- Downstream accuracy (RAG): matches full HNSW at 25.5% (vs 17.9% for PQ-only)
- Latency: ~10% end-to-end overhead for RAG (generation dominates)
- Platforms: RTX 4090 server, M1 Mac

## Significance

LEANN introduces a paradigm shift: instead of progressively compressing vectors, eliminate vector storage entirely by exploiting the observation that ANN search only touches a tiny fraction of nodes. This is particularly compelling for RAG where the encoder model is already available. The "recompute rather than store" philosophy is novel in the ANN space, though it's been used in other domains (e.g., learned index recomputation).

## Limits And Open Questions

- Requires the embedding model to be available at query time — adds GPU compute dependency and latency.
- Only works when original data (text, images) is stored alongside the index — not applicable to third-party embedding datasets.
- Embedding model updates would change the vector space, requiring index rebuild.
- Graph pruning thresholds need per-dataset tuning; aggressive pruning can create disconnected components.

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [HNSW](../entities/hnsw.md)
