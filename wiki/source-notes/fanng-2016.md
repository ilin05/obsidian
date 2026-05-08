---
id: fanng-2016
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - graph-index
  - proximity-graph
  - rng
source_count: 1
sources:
  - raw/sources/papers/fanng-2016.pdf
related:
  - fanng
  - monotonic-relative-neighborhood-graph
  - proximity-graph-theory-for-ann
  - hnsw
  - nsg
confidence: high
---

# FANNG: Fast Approximate Nearest Neighbour Graphs

## Summary

Harwood and Drummond propose FANNG, a graph ANN index for feature vectors built around an occlusion-style neighbor pruning rule. The method constructs a directed graph intended to support efficient greedy search while avoiding excessive local degree.

## Key Contributions

- Uses an occlusion rule: a candidate neighbor can be suppressed when a closer intermediate point also lies closer to that candidate, making the direct edge redundant for local search.
- Connects practical graph ANN pruning to relative-neighborhood-graph intuition.
- Targets fast high-recall search over high-dimensional image features.
- Includes GPU-oriented implementation results, making it an early bridge between graph ANN and accelerator execution.

## Relation To Proximity Graph Theory

FANNG is important because its pruning rule resembles RNG/MRNG reasoning without presenting itself as the same exact theoretical object. It helps explain why later methods such as [NSG](../entities/nsg.md), SSG/NSSG, and Vamana focus on diversified neighbor sets rather than simply keeping the closest k neighbors.

## Limits And Open Questions

- The exact searchability guarantees are weaker than the later MRNG theory used to explain NSG-style monotonicity.
- GPU results are implementation-specific and should not be treated as a general hardware-aware ANN benchmark.
- Its role in modern comparisons is mainly as an RNG-like pruning predecessor.

## Related Pages

- [FANNG](../entities/fanng.md)
- [Monotonic Relative Neighborhood Graph](../entities/monotonic-relative-neighborhood-graph.md)
- [Proximity Graph Theory for ANN](../topics/proximity-graph-theory-for-ann.md)
- [NSG](../entities/nsg.md)
