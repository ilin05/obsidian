---
id: patience-in-proximity
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - method
  - ann
  - hnsw
  - early-termination
source_count: 1
sources:
  - raw/patience.pdf
related:
  - patience-in-proximity-2025
  - hnsw
  - approximate-nearest-neighbor-search
confidence: medium
---

# Patience in Proximity

## Profile

Patience in Proximity is an HNSW traversal heuristic that introduces saturation-based early termination to reduce search-time computation.

## Why It Matters

- It targets a practical bottleneck inside HNSW search loops with a minimal algorithmic change.
- It is useful for latency-sensitive dense-retrieval settings where small quality loss can be acceptable.

## Related Pages

- [Patience in Proximity Source Note](../source-notes/patience-in-proximity-2025.md)
- [HNSW](hnsw.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)

