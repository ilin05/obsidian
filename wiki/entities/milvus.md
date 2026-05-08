---
id: milvus
type: entity
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - ann
  - systems
  - vector-database
  - production
  - zilliz
source_count: 1
sources:
  - raw/sources/papers/milvus-2021.pdf
related:
  - faiss
  - approximate-nearest-neighbor-search
confidence: high
---

# Milvus

**A Purpose-Built Vector Data Management System**  
Zilliz (with Purdue University), SIGMOD 2021  
Open-source: https://github.com/milvus-io/milvus  
Foundation model: LF AI & Data Foundation

## What It Is

Milvus is a production-grade, open-source vector DBMS designed for large-scale, dynamic, heterogeneous AI workloads. Built on top of Faiss as its core search engine, it adds dynamic data management, attribute filtering, multi-vector queries, heterogeneous CPU/GPU computing, and distributed deployment. Deployed by hundreds of organizations.

## Key Capabilities

| Feature | Support |
|---|---|
| Billion-scale data | Yes |
| Dynamic data | Yes (insertions + deletions) |
| GPU acceleration | Yes |
| Attribute filtering | Yes (5 strategies) |
| Multi-vector query | Yes (fusion + iterative merging) |
| Distributed system | Yes (sharding + broadcasting) |

## Architecture

- **Query engine:** cache-aware batching (m queries × n vectors fit L3), SIMD auto-select (SSE/AVX/AVX512), GPU kernel dispatch.
- **Storage engine:** LSM-tree with MemTable → immutable segments; snapshot isolation; multi-filesystem (S3/HDFS).
- **GPU engine:** CUDA-based, SIMD-instruction-set auto-detection at runtime via hook-based function pointer.

## Optimization Insights

**Cache-aware query batching:** partitions m queries × n data vectors into blocks of size s fitting L3 cache. Achieves 1.5–2.7× over Faiss by reusing database vectors across queries. This is a useful CPU-local precedent for batch-oriented ANN execution.

**SIMD auto-selection:** Milvus detects available SIMD extensions at runtime and binds function pointers accordingly.

## Scope and Limitations

- Supports dynamic updates.
- Assumes data fits in distributed DRAM across multiple nodes; does not address CXL memory tiers.
- Full DBMS with query optimization, scheduling, concurrency.

## Relation to ANN Systems

- Milvus is the **canonical production vector database reference** for related work.
- Milvus shows what a complete system looks like when built on FAISS+HNSW.
- Milvus cache-aware batching is a useful CPU-side precedent for batch-oriented vector search.
