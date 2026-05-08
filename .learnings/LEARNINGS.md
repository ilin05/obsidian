# Learnings

Corrections, insights, and knowledge gaps captured during development.

**Categories**: correction | insight | knowledge_gap | best_practice

---

## [LRN-20260408-001] correction

**Logged**: 2026-04-08T19:20:00+08:00
**Priority**: medium
**Status**: pending
**Area**: docs

### Summary
Do not use external-paper reproduction docs as evidence when reviewing the user's own CXL-HNSW project document.

### Details
I incorrectly cited `CXL_ANNS_REPRODUCTION.md` to challenge the maturity of a baseline in `raw/CXL_HNSW_CODE_ARCHITECTURE_ANALYSIS copy.md`. The user clarified that this reproduction doc belongs to another paper's reproduction track, while the current project framing is already captured in the vault knowledge graph. For project review, evidence should be restricted to the project's own knowledge-graph pages and the relevant implementation path.

### Suggested Action
When reviewing thesis/project documents in this vault, separate three evidence classes explicitly: current project docs in the knowledge graph, current implementation, and external reproduction/material for other papers. Do not mix them unless the user asks for cross-project comparison.

### Metadata
- Source: user_feedback
- Related Files: raw/CXL_HNSW_CODE_ARCHITECTURE_ANALYSIS copy.md, wiki/overview/cxl-vector-project-overview.md, wiki/analyses/cxl-vector-thesis-positioning.md
- Tags: review-scope, evidence-boundary, thesis

---
