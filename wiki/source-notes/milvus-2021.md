---
id: milvus-2021
type: source-note
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - systems
  - vector-database
  - production
source_count: 1
sources:
  - raw/sources/papers/milvus-2021.pdf
related:
  - milvus
  - faiss
  - approximate-nearest-neighbor-search
confidence: high
---

# Milvus: A Purpose-Built Vector Data Management System

**Authors:** Jianguo Wang et al. (Zilliz, Purdue University)  
**Venue:** SIGMOD 2021  
**DOI:** 10.1145/3448016.3457559

## Core Contribution

Milvus is a production vector DBMS designed for large-scale, dynamic, heterogeneous AI workloads. It extends Faiss with: dynamic data management, attribute filtering, multi-vector queries, heterogeneous computing optimization, and distributed deployment. Deployed by hundreds of organizations.

## System Architecture

Three main components:
- **Query engine:** CPU/GPU-aware query processing with SIMD and cache optimizations.
- **GPU engine:** hybrid index management with CUDA kernels.
- **Storage engine:** LSM-tree based dynamic management + snapshot isolation.

## Index Support

Quantization-based: IVF_FLAT, IVF_SQ8, IVF_PQ.  
Graph-based: HNSW, RNSG.  
Milvus uses Faiss as its core search engine with significant CPU/GPU enhancements.

## Cache-Aware Optimization

Key insight from Faiss: one thread per query, all threads stream the full dataset → high LLC miss rate. Milvus partitions m queries × n database vectors into blocks of size s fitting in L3 cache:

s = L3_size / (d × sizeof(float) + t × k × (sizeof(int64) + sizeof(float)))

- 1.5–2.7× improvement over Faiss on CPU due to reuse of database vectors across queries.
- Directly relevant: Milvus shows how batching multiple queries together can amortize data movement and improve cache behavior.

## SIMD Optimizations

Auto-selects at runtime: SSE, AVX, AVX2, AVX512 (hook-based function pointer dispatch). Avoids manually selecting SIMD width at compile time.

## Dynamic Data Management

- LSM-tree structure: insertions/deletions go to in-memory MemTable → flushed to immutable segments.
- Snapshot isolation: concurrent reads see consistent snapshot.
- Merge policy: segments merged until configurable size limit (e.g., 1GB).
- Milvus supports dynamic updates and full DBMS behavior rather than only standalone ANN serving.

## Distributed System

- Data sharding across nodes; query broadcasting.
- Amazon S3, HDFS multi-storage support.
- Segment is the basic unit of scheduling, buffering, and searching.

## Attribute Filtering

Five strategies (A–E) for combining attribute and vector constraints. Cost-based selection between strategies depending on attribute selectivity. Milvus Strategy E (partition-based) most efficient when attribute predicate is moderately selective.

## Multi-Vector Query

Two algorithms: vector fusion (inner product decomposable similarity) and iterative merging (general similarity, uses NRA top-k algorithm).

## System Comparison (Table 1)

Only Milvus simultaneously supports: billion-scale data, dynamic data, GPU, attribute filtering, multi-vector query, distributed system.

## Relevance to ANN Systems

- Milvus is the canonical production vector database reference.
- The cache-aware batch query optimization in Milvus is a useful CPU-local precedent for batch-oriented search.
- Milvus's HNSW + quantized index combination is relevant to graph traversal plus compressed scoring designs.

## Open Questions

- How does Milvus's segment-based scheduling compare to other batch-oriented query scheduling approaches?
