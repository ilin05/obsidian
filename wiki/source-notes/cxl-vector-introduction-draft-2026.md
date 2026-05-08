---
id: cxl-vector-introduction-draft-2026
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - cxl-vector
  - introduction
  - paper-writing
source_count: 1
sources:
  - vldb-paper-intro-draft.md
related:
  - cxl-vector-vldb-paper
  - cxl-vector
  - cxl-vector-vldb-positioning
confidence: high
---

# CXL-Vector Introduction Draft

## Source

`vldb-paper-intro-draft.md`

## Summary

The introduction draft sets the paper narrative as: capacity wall, CXL as missing tier, naive CXL-HNSW collapse, three failure modes, distinction from CXL-ANNS, scheduling insight, system overview, and contributions.

## Durable Narrative Points

- CXL is framed as fast enough for pointer-chasing graph traversal and large enough for billion-scale embedding tables.
- The failure of naive CXL-HNSW is attributed to PCIe bandwidth saturation, not compute or graph complexity.
- The three failure modes are unconstrained raw-vector access, single-query blocking, and unmanaged thread-level bandwidth consumption.
- The paper explicitly distinguishes itself from CXL-ANNS: CXL-ANNS asks what future endpoint compute can do; CXL-Vector asks what commodity CXL memory can do in pure software.
- The central sentence is that serving HNSW on commodity CXL is primarily a scheduling problem once raw-vector access is bounded.

## Writing Risks

- Several claims are numerically specific and need stable artifacts.
- The introduction is currently long for a 1.5-page PVLDB budget.
- The CXL-ANNS positioning is load-bearing; if moved to related work, reviewers may still ask the distinction early.
- The draft should add a crisp line about why HNSW is the chosen graph family relative to Vamana/NSG.

