---
id: vbase
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - vector-database
  - query-processing
source_count: 1
sources:
  - raw/sources/papers/vbase-2023.pdf
related:
  - milvus
  - spann
  - rnsg
  - approximate-nearest-neighbor-search
confidence: high
---

# VBASE

## Profile

VBASE is an OSDI 2023 PostgreSQL-based vector database system that unifies online vector similarity search with relational query processing through relaxed monotonicity.

## Main Ideas

- Treat vector index traversal as approximately monotonic in a two-phase pattern.
- Replace TopK-only vector access with an iterator-style interface.
- Use generalized termination checks to avoid guessing a temporary overfetch K.
- Support hybrid vector/scalar queries, multi-column TopK, range filter, and vector joins.

## Position In The Vault

VBASE is a query-semantics reference rather than a memory-tier design. It is useful when discussing whether billion-scale ANN systems only optimize pure TopK or can serve production database queries with filters and joins.

[RNSG](rnsg.md) is now the closest graph-theoretic neighbor in the vault for filtered vector search: VBASE focuses on vector-relational query processing through relaxed monotonicity, while RNSG focuses on range-filtered graph-index structure.

## Key Evidence

For complex queries beyond plain TopK, VBASE reports 100x-1000x lower latency than baseline vector systems under similar accuracy. For a join query, it reports about 7900x over PostgreSQL brute-force scan with recall 0.9992.

## Related Pages

- [VBASE Source Note](../source-notes/vbase-2023.md)
- [Milvus](milvus.md)
- [SPANN](spann.md)
- [RNSG](rnsg.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
