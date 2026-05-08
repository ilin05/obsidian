---
id: spfresh-2023
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - billion-scale
  - updates
  - ssd
source_count: 1
sources:
  - raw/sources/papers/spfresh-2023.pdf
related:
  - spfresh
  - spann
  - diskann
  - milvus
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# SPFresh: Incremental In-Place Update for Billion-Scale Vector Search

## Bibliographic Note

Yuming Xu, Hengyu Liang, Jin Li, Shuotao Xu, Qi Chen, Qianxi Zhang, Cheng Li, Ziyue Yang, Fan Yang, Yuqing Yang, Peng Cheng, and Mao Yang. 2023. *SPFresh: Incremental In-Place Update for Billion-Scale Vector Search*. SOSP 2023. DOI: `10.1145/3600006.3613166`.

Primary PDF: <https://arxiv.org/pdf/2410.14452>.

The system name is [SPFresh](../entities/spfresh.md).

## Core Problem

SPFresh targets fresh updates for billion-scale disk-based vector indexes. Existing systems often accumulate inserts in secondary indexes and periodically merge or globally rebuild the main index. That LSM-style approach can preserve write throughput but causes fluctuating search latency, accuracy changes, and high rebuild resource cost.

The paper focuses on the case where vector data changes continuously and the index must support search and update online.

## Design

SPFresh is built on a SPANN-style balanced cluster index and introduces **LIRE**, a lightweight incremental rebalancing protocol.

- **In-place updates:** insert and delete vectors inside the primary on-disk index instead of accumulating a separate delta index.
- **Posting split/merge:** keep partition sizes balanced as updates accumulate.
- **Nearest partition assignment:** preserve the property that a vector belongs to the nearest posting, maintaining index quality under distribution shift.
- **Local reassignment:** reassign only a small neighborhood of boundary vectors near affected postings.
- **Two-stage feed-forward pipeline:** decouple foreground update from background split/reassign work so update handling does not block search.

## Evaluation Facts

- The abstract reports that SPFresh uses only 1% of DRAM and less than 10% of cores at peak compared with the state of the art for a billion-scale disk-based index with a 1% daily update rate.
- The paper reports that DiskANN global rebuild needs an additional 1000 GB memory and 32 cores, while SPFresh uses 10 GB memory and 2 cores for the comparable maintenance path.
- SPFresh reports 2.41x lower average tail latency than DiskANN under the fresh-update workload.
- The system reaches peak 4K QPS search throughput and 2K QPS update throughput on a single NVMe SSD with 15 cores.
- Under simulated real-world updates, SPFresh keeps P99.9 latency around 4 ms, while DiskANN can exceed 20 ms during global rebuilds and SPANN+ can grow beyond 10 ms as postings become skewed.
- In billion-scale stress tests, SPFresh reaches NVMe IOPS limits while keeping stable performance and resource utilization.

## Relevance To Billion-Scale Vector Search

SPFresh fills a gap that most static ANN benchmarks ignore: update freshness. It is especially important when evaluating claims about production vector databases, RAG stores, or recommendation systems where newly inserted vectors must become searchable quickly.

The paper also provides a useful systems distinction: cluster-based indexes may be friendlier to local updates than graph-based indexes, but naive in-place updates still degrade partition balance and recall unless the index is actively rebalanced.

## Follow-up Questions

- How should fresh-update workloads be represented in billion-scale ANN benchmarks?
- Can LIRE-style local rebalancing combine with GPU or SmartSSD search paths?
- What update guarantees are required for RAG workloads: immediate visibility, bounded staleness, or batch freshness?
