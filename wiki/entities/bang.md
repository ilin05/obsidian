---
id: bang
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - gpu
  - graph-index
source_count: 1
sources:
  - raw/sources/papers/bang-2024.pdf
related:
  - rummy
  - gustann
  - diskann
  - product-quantization
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# BANG

## Profile

BANG is a single-GPU graph-based ANNS system for billion-scale datasets that exceed GPU memory. It keeps the graph on host memory while using compressed vectors on the GPU for distance computation.

## Main Ideas

- Store the graph index on CPU memory and fetch neighbor lists on demand.
- Keep PQ-compressed vectors on GPU to make distance computation feasible in HBM.
- Split graph search into CPU/GPU phases and overlap communication with computation.
- Use optimized GPU kernels for distance calculation, sorting, worklist updates, and prefetching.

## Position In The Vault

BANG is a key out-of-GPU-memory graph baseline. It is related to RUMMY at the CPU-GPU scheduling layer and to DiskANN/GustANN at the graph-search layer.

## Key Evidence

The paper reports 50x-400x higher throughput than competing methods at recall 0.9 on three billion-scale datasets using one NVIDIA A100 GPU.

## Related Pages

- [BANG Source Note](../source-notes/bang-2024.md)
- [RUMMY](rummy.md)
- [GustANN](gustann.md)
- [DiskANN](diskann.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
