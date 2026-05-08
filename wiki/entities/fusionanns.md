---
id: fusionanns
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - gpu
  - ssd
source_count: 1
sources:
  - raw/sources/papers/fusionanns-2025.pdf
related:
  - gustann
  - spann
  - diskann
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# FusionANNS

## Profile

FusionANNS is a FAST 2025 billion-scale ANNS system that combines SSD storage with one entry-level GPU. Its central mechanism is CPU/GPU collaborative filtering and reranking to reduce movement across SSD, CPU memory, and GPU HBM.

## Main Ideas

- Store raw vectors on SSD, PQ-compressed vectors in GPU HBM, and vector IDs plus navigation graph in CPU memory.
- Send vector IDs rather than vector contents to the GPU for filtered distance computation.
- Apply mini-batch heuristic reranking to avoid unnecessary raw-vector I/O.
- Deduplicate raw-vector SSD requests by exploiting compact storage of similar vectors.

## Position In The Vault

FusionANNS is important for memory-tiered ANN because it makes the accurate raw-vector stage explicit and then optimizes its I/O. It is an SSD+GPU design whose reranking structure is a useful template for thinking about coarse-to-exact search paths.

## Key Evidence

At recall@10 = 90%, the paper reports 9.4x-13.1x higher QPS and 5.7x-8.8x higher cost efficiency than SPANN, and 2x-4.9x higher QPS plus 2.3x-6.8x higher cost efficiency than RUMMY.

## Related Pages

- [FusionANNS Source Note](../source-notes/fusionanns-2025.md)
- [GustANN](gustann.md)
- [SPANN](spann.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
