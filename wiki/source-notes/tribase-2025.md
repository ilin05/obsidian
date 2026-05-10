---
id: tribase-2025
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - ann
  - cluster-based-index
  - triangle-inequality
  - pruning
  - in-memory
source_count: 1
sources:
  - raw/sources/papers/tribase-2025.pdf
related:
  - approximate-nearest-neighbor-search
  - faiss
confidence: high
---

# TriBase: A Vector Data Query Engine for Reliable and Lossless Pruning Compression using Triangle Inequalities

Qian Xu, Juan Yang, Feng Zhang, Junda Pan, Kang Chen, Youren Shen, Amelie Chi Zhou, and Xiaoyong Du, SIGMOD 2025 (Renmin University, Tsinghua University).

## Summary

TriBase improves cluster-based ANNS by using triangle inequalities for lossless vector pruning. The core observation: existing cluster-based indexes treat all vectors within selected clusters equally, leading to unnecessary distance computations. TriBase refines cluster granularity through multi-level partitioning and applies both distance-based and angle-based triangle inequalities to prune vectors that cannot be among the top-k results. Integrated into Faiss, it achieves up to 10× speedup with >99.4% pruning ratio.

## Key Contributions

- **Fine-grained cluster index**: Subdivides clusters into smaller partitions using diverse distance/angle metrics, enabling more precise pruning conditions.
- **Dual triangle inequality pruning**: Leverages both distance-based (d(A,B) ≤ d(A,C) + d(C,B)) and angle-based (∠AOB + ∠BOC ≥ ∠AOC) triangle inequalities for pruning.
- **Flexible pruning intensity**: Scaling of triangle inequality bounds allows adjusting the pruning-aggressiveness vs accuracy trade-off.
- **Faiss integration**: Pruning modules are integrated into Faiss as a drop-in optimization, demonstrating composability with existing systems.

## Algorithm Details

### Index Construction
1. Standard k-means clustering to form initial clusters.
2. Within each cluster, apply multi-level partitioning based on distance/angle metrics.
3. For each sub-partition, store representative points and pre-computed bounds for triangle inequality checks.

### Query Processing
1. Compute query-to-centroid distances; select nearest clusters.
2. For each selected cluster, use distance-based triangle inequality to prune sub-partitions.
3. For remaining vectors, use angle-based triangle inequality for further pruning.
4. Compute exact distances only for vectors that survive both pruning stages.
5. Update pruning bounds dynamically as better candidates are found.

## Reported Results

- Pruning ratio: up to 99.4% vectors eliminated without accuracy loss
- Speedup: up to 10× vs Faiss (3.11× average across 10 datasets)
- Datasets: msong, sift, glove, deep, gist, and others
- Distance computation accounts for 99.7% of total search time in baseline

## Significance

TriBase addresses a fundamental inefficiency in cluster-based ANNS: the assumption that all vectors in a cluster are equally worthy of distance computation. By applying triangle inequalities systematically, it achieves lossless pruning — no accuracy degradation — which is rare among ANN acceleration techniques. The dual use of distance and angle inequalities is novel and well-motivated by high-dimensional geometry.

## Limits And Open Questions

- Pruning effectiveness degrades with increasing dimensionality due to the curse of dimensionality making triangle bounds looser.
- Additional metadata storage for pre-computed bounds (not quantified in the paper at large scale).
- Only evaluated on in-memory cluster-based indexes; applicability to graph-based indexes is not explored.
- The angle-based inequality requires computing angles, which may add overhead in lower dimensions.

## Related Pages

- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [FAISS](../entities/faiss.md)
