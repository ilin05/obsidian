---
id: spfresh
type: entity
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - updates
  - ssd
source_count: 1
sources:
  - raw/sources/papers/spfresh-2023.pdf
related:
  - spann
  - diskann
  - svfusion
  - milvus
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# SPFresh

## Profile

SPFresh is a SOSP 2023 disk-based vector search system for incremental in-place updates at billion scale. It is built around LIRE, a lightweight incremental rebalancing protocol for SPANN-style partitions.

## Main Ideas

- Avoid secondary-index accumulation and global rebuilds.
- Insert and delete directly in the primary index.
- Split, merge, and locally rebalance postings as distributions shift.
- Keep update maintenance off the search critical path through a two-stage pipeline.

## Position In The Vault

SPFresh is the main update-freshness reference for billion-scale vector search. It should be cited whenever a system claims production readiness but only evaluates static indexes.

[SVFusion](svfusion.md) is adjacent on the dynamic-workload axis, but it studies CPU-GPU-disk streaming search rather than SPANN-style disk-resident cluster maintenance.

## Key Evidence

The paper reports 2.41x lower average tail latency than DiskANN under fresh updates, 4K QPS search plus 2K QPS update throughput on one NVMe SSD, and stable P99.9 latency around 4 ms in its update simulation.

## Related Pages

- [SPFresh Source Note](../source-notes/spfresh-2023.md)
- [SVFusion](svfusion.md)
- [SPANN](spann.md)
- [DiskANN](diskann.md)
- [Milvus](milvus.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Second-tier Memory for Vector Search](../topics/second-tier-memory-for-vector-search.md)
