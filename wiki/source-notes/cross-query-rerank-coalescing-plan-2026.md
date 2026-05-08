---
id: cross-query-rerank-coalescing-plan-2026
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - cxl-vector
  - rerank
  - scheduling
source_count: 1
sources:
  - cross-query-rerank-coalescing-plan.md
related:
  - cross-query-rerank-coalescing
  - cxl-vector
confidence: high
---

# Cross-Query Rerank Coalescing Plan

## Source

`cross-query-rerank-coalescing-plan.md`

## Summary

This design note proposes a new mechanism for CXL-Vector: deduplicate exact-rerank raw-vector fetches across queries in the same morsel. The proposed mechanism is intended to strengthen the paper's CXL-specific novelty by showing that morsel-local batch execution enables optimizations unavailable to single-query systems.

## Core Mechanism

Current rerank path:

- each query selects its own rerank candidates;
- each query independently prefetches raw vectors from CXL;
- duplicate candidates across queries cause duplicate CXL fetches.

Proposed path:

- collect all rerank candidates across the morsel;
- deduplicate by `node_id`;
- prefetch each unique raw vector once;
- scatter exact-distance computation back to each owner query.

## Required First Measurement

Before implementation, measure:

- total rerank candidates per morsel;
- unique rerank candidates per morsel;
- overlap ratio by dataset and morsel size.

The design note sets a decision threshold: below 3% overlap, do not pursue as a major contribution; 5-20% overlap is worth implementation; above 20% would be strong evidence.

## Implementation Direction

- Start with sort plus run-length grouping over `(node_id, query_idx)` pairs.
- Add `EXP_RERANK_COALESCE` behind a default-off flag.
- Implement scatter-compute exact distance carefully enough to preserve bit-exact recall.
- Evaluate QPS, PCIe bandwidth, latency distribution, overlap histogram, and dedup overhead.

