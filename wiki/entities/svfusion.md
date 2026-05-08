---
id: svfusion
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - gpu
  - streaming
  - updates
source_count: 1
sources:
  - raw/sources/papers/svfusion-2026.pdf
related:
  - bang
  - spfresh
  - fusionanns
  - gustann
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: medium
---

# SVFusion

## Profile

SVFusion is a 2026 CPU-GPU-disk cooperative system for streaming vector search. It targets workloads where queries, insertions, and deletions run concurrently and the searchable vectors exceed GPU memory capacity.

## Main Ideas

- Keep hot vectors and subgraphs uncompressed in GPU HBM.
- Keep the complete graph in CPU memory or disk-backed storage.
- Decide per cache miss whether to transfer a vector to GPU or compute its distance on CPU.
- Use graph topology and workload history for vector placement.
- Use lazy deletion, affected-vertex repair, reverse-edge insertion, and multi-version synchronization for update consistency.

## Position In The Vault

SVFusion connects three ANN branches that were previously mostly separate in the vault:

- GPU graph search beyond HBM, represented by [BANG](bang.md).
- Update-fresh vector systems, represented by [SPFresh](spfresh.md).
- SSD/GPU or CPU/GPU collaborative data movement, represented by [GustANN](gustann.md), [FusionANNS](fusionanns.md), and [RUMMY](rummy.md).

Its main value is methodological: it shows that dynamic ANN systems need to report recall, throughput, insertion throughput, latency, and cache-miss behavior together.

## Key Evidence

The paper reports up to 9.5x higher search throughput, 71.8x higher insertion throughput, and 1.3x to 50.7x lower latency versus its baseline set while maintaining high recall under streaming workloads.

## Caveat

This page is based on arXiv v1 of the paper. Treat SVFusion as a high-value emerging system and candidate baseline, but verify venue status and implementation availability before using it as a settled canonical result.

## Related Pages

- [SVFusion Source Note](../source-notes/svfusion-2026.md)
- [BANG](bang.md)
- [SPFresh](spfresh.md)
- [FusionANNS](fusionanns.md)
- [GustANN](gustann.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
