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
- Directly relevant: CXL-Vector's morsel-driven batching pursues a similar principle — batch multiple queries together to amortize remote memory fetch cost.

## SIMD Optimizations

Auto-selects at runtime: SSE, AVX, AVX2, AVX512 (hook-based function pointer dispatch). Avoids manually selecting SIMD width at compile time.

## Dynamic Data Management

- LSM-tree structure: insertions/deletions go to in-memory MemTable → flushed to immutable segments.
- Snapshot isolation: concurrent reads see consistent snapshot.
- Merge policy: segments merged until configurable size limit (e.g., 1GB).
- **Key distinction from CXL-Vector:** Milvus supports dynamic updates; CXL-Vector is explicitly a read-only serving runtime.

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

## Relevance to CXL-Vector

- Milvus is the canonical production vector database reference. CXL-Vector should be positioned as a CXL-aware serving runtime (complementary to, not competing with, full systems like Milvus).
- The cache-aware batch query optimization in Milvus is the CPU-local precursor to CXL-Vector's morsel-driven batching, which extends the same principle to the DRAM+CXL tier.
- Milvus's HNSW + quantized index combination (HNSW for graph traversal, SQ8 for compression) mirrors CXL-Vector's design; the CXL dimension adds a new constraint that Milvus doesn't address.

## Open Questions

- Would a Milvus-like production system benefit from CXL-Vector's layout and scheduling techniques when scaled beyond single-node DRAM?
- How does Milvus's segment-based scheduling compare to CXL-Vector's morsel-driven query scheduling in throughput characteristics?
