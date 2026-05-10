---
id: leann
type: entity
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

# LEANN

Storage-efficient vector index that recomputes embeddings on-the-fly instead of storing them (Wang et al., UC Berkeley, under review 2025).

## Core Idea

Two pillars: (1) **Embedding recomputation** — re-encode original data items at query time using the embedding model, since graph search visits very few nodes; (2) **High-degree preserving graph pruning** — aggressively prune low-degree nodes while preserving hub connectivity. Two-level search uses PQ for cheap pruning, then GPU-batched encoder inference for exact reranking.

## Key Properties

- Index size: up to 50× smaller than conventional HNSW+PQ
- Storage: down to 5% of original data size
- Downstream accuracy (RAG): matches uncompressed baseline
- Requires embedding model at query time (GPU dependency)
- Not yet peer-reviewed

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [LEANN Source Note](../source-notes/leann-2025.md)
- [HNSW](hnsw.md)
