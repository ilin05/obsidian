---
id: monotonic-relative-neighborhood-graph
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - concept
  - ann
  - graph-index
  - proximity-graph
source_count: 3
sources:
  - raw/sources/papers/nsg-2019.pdf
  - raw/sources/papers/monotonic-proximity-graphs-2021.pdf
  - raw/sources/papers/satellite-system-graph-2019.pdf
related:
  - nsg
  - satellite-system-graph
  - rnsg
  - proximity-graph-theory-for-ann
  - approximate-nearest-neighbor-search
confidence: medium
---

# Monotonic Relative Neighborhood Graph

## Profile

Monotonic Relative Neighborhood Graph (MRNG) is a theoretical proximity-graph model for graph-based ANN search. It tries to preserve monotonic paths: from a starting point, local graph traversal can move through nodes that get progressively closer to the target.

## Why It Matters

- [NSG](nsg.md) is explicitly motivated as a practical approximation of MRNG.
- MRNG gives a clean explanation for why relative-neighborhood pruning can remove many edges while preserving searchability.
- Exact MRNG is too expensive to build at scale, so practical systems use bounded-degree and candidate-pool approximations.
- Later methods such as [Satellite System Graph](satellite-system-graph.md) and [RNSG](rnsg.md) modify or extend the monotonic-proximity idea for unindexed queries and range-filtered queries.

## Key Caution

MRNG is best treated as a theoretical target, not a deployable index. The important systems question is how much monotonic navigability survives after approximate candidate generation, degree caps, storage layout constraints, and hardware-tier placement.

## Related Pages

- [MRNG Generalization Source Note](../source-notes/monotonic-proximity-graphs-2021.md)
- [NSG](nsg.md)
- [Satellite System Graph](satellite-system-graph.md)
- [RNSG](rnsg.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
