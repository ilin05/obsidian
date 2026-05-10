---
id: tribase
type: entity
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - cluster-based-index
  - triangle-inequality
  - pruning
source_count: 1
sources:
  - raw/sources/papers/tribase-2025.pdf
related:
  - approximate-nearest-neighbor-search
  - faiss
confidence: high
---

# TriBase

Vector query engine using triangle inequalities for lossless pruning in cluster-based ANNS (Xu et al., SIGMOD 2025).

## Core Idea

Refine cluster-based indexes with multi-level partitioning, then apply both distance-based and angle-based triangle inequalities to prune vectors that provably cannot be in the top-k. Lossless: no accuracy degradation. Integrated into Faiss as a drop-in module.

## Key Properties

- Pruning ratio: >99.4% vectors eliminated
- Speedup: up to 10× vs Faiss (3.11× average)
- Pruning is lossless (no accuracy cost)
- Distance computation is 99.7% of baseline search time

## Related Pages

- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [TriBase Source Note](../source-notes/tribase-2025.md)
- [FAISS](faiss.md)
