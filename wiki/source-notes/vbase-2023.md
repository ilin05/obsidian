---
id: vbase-2023
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - vector-database
  - query-processing
  - relational
source_count: 1
sources:
  - raw/sources/papers/vbase-2023.pdf
related:
  - vbase
  - milvus
  - spann
  - approximate-nearest-neighbor-search
confidence: high
---

# VBASE: Unifying Online Vector Similarity Search and Relational Queries via Relaxed Monotonicity

## Bibliographic Note

Qianxi Zhang, Shuotao Xu, Qi Chen, Guoxin Sui, Jiadong Xie, Zhizhen Cai, Yaoqi Chen, Yinxuan He, Yuqing Yang, Fan Yang, Mao Yang, and Lidong Zhou. 2023. *VBASE: Unifying Online Vector Similarity Search and Relational Queries via Relaxed Monotonicity*. OSDI 2023.

Primary page: <https://www.usenix.org/conference/osdi23/presentation/zhang-qianxi>.

The system name is [VBASE](../entities/vbase.md).

## Core Problem

VBASE targets online vector queries that combine vector similarity with relational operators such as filters, multi-column vector ranking, range filters, and joins.

Existing vector systems often expose a TopK-only interface. For filtered or hybrid queries, the system must guess a larger temporary K before applying relational operators. If the guessed K is too small, recall suffers; if it is too large, latency increases.

## Design

VBASE's central abstraction is **relaxed monotonicity**. The paper argues that well-designed vector indexes follow a two-phase traversal pattern: they first move toward the target region approximately, then progressively move away. This is weaker than B-tree monotonicity but strong enough to support generalized termination conditions.

The system uses this property to build a PostgreSQL-based unified query engine:

- Replace the TopK-only interface with a `Next`-style iterator interface.
- Use relaxed monotonicity checks to decide when index traversal can stop.
- Support TopK, TopK plus filters, multi-column TopK, range filter, and vector join queries.
- Preserve equivalence to an optimal TopK-only execution plan while avoiding static overfetch guesses.

## Evaluation Facts

- VBASE is implemented in PostgreSQL with about 2000 additional lines of code.
- The evaluation uses eight complex SQL queries over a hybrid vector/scalar recipe dataset.
- For complex queries beyond plain TopK, VBASE reports 100x-1000x lower query latency than baseline systems while preserving similar or better recall.
- For multi-column TopK queries Q4-Q6, VBASE reports 200x-300x lower latency than Milvus and 96%+ recall.
- For join-based vector query Q8, VBASE reports about 7900x speedup over PostgreSQL brute-force scan with recall 0.9992.
- VBASE also evaluates a SPANN-backed configuration, showing the abstraction can support both graph-based and partition-based vector indexes.

## Relevance To Billion-Scale Vector Search

VBASE is not primarily a memory-tier paper. Its role in this vault is query semantics: many billion-scale ANN systems optimize plain TopK search, but production vector databases need filtered and relational queries.

It should be cited when discussing how ANN indexes expose results to a database engine, especially when a system claims online serving, filtered search, or SQL integration.

## Follow-up Questions

- Which billion-scale systems support VBASE-like hybrid queries rather than only pure TopK?
- Can relaxed monotonicity interact with approximate reranking or compressed candidate stages?
- How should benchmark suites represent filtered queries and joins for billion-scale vector databases?
