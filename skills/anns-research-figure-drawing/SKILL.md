---
name: anns-research-figure-drawing
description: Use when drawing, redesigning, reviewing, or iterating research-paper figures for ANNS, vector search, vector databases, graph indexes, quantization, SSD/GPU/CXL/RDMA/NDP systems, or empirical-evaluation sections.
---

# ANNS Research Figure Drawing

## Overview

Use repository figures as design references, not as templates to copy blindly. Every figure must serve one paper claim and must match the section's technical content.

## Workflow

1. Read [FIGURE_CACHE.md](../../FIGURE_CACHE.md).
2. Classify the requested figure: system architecture, data layout, query workflow, algorithm intuition, taxonomy, characterization, Pareto evaluation, ablation, sensitivity, or dynamic workload.
3. Pick 2-4 reference figures from the cache that match the claim.
4. Draft the figure with vector-editable source when possible.
5. Review against the checklist below before returning it.

## System Figure Rules

- Show concrete components, maintained state, data placement, and movement.
- Separate offline build, online query, update/maintenance, and background repair when they differ.
- Label memory tiers and compute actors: CPU, GPU, HBM, DRAM, SSD, CXL, RDMA, SmartSSD, host/device.
- Use arrows only for real control flow or data flow.
- If comparing baseline vs proposed design, use parallel panels with identical visual vocabulary.
- Avoid making performance claims inside architecture figures unless the section has evidence.

## Experiment Figure Rules

- Use matched recall for speed claims.
- Include metric, units, dataset/workload, and baseline identity.
- Use log scale only when needed and label it visibly.
- Prefer Pareto curves for recall-QPS or recall-latency tradeoffs.
- Prefer grouped bars or stacked breakdowns for mechanism attribution.
- Prefer sensitivity curves for thread count, batch size, memory ratio, device count, beam width, candidate set, or rerank budget.
- Never plot unsupported or fabricated numbers.

## Review Checklist

- Claim: the figure answers one clear question.
- Correctness: states, arrows, algorithms, and tiers match the paper text and implementation context.
- Novelty: the figure exposes what differs from HNSW, NSG, DiskANN, SPANN, FAISS, ScaNN, CXL-ANNS, BANG, or other closest baselines.
- Evaluation: axes, units, recall definition, k, dataset, hardware tier, and baseline labels are unambiguous.
- Readability: no tiny text, unexplained acronyms, overloaded colors, or crossing arrows that hide the mechanism.
- Caption: states what to learn, not just what is drawn.

## Common Mistakes

- Mixing architecture, workflow, and performance result in one overloaded figure.
- Using recall-QPS curves without matched-recall interpretation.
- Drawing memory tiers but not showing which data structure resides where.
- Showing a cache/prefetch/scheduler without the decision rule or maintained state.
- Showing a graph algorithm without construction, seed acquisition, pruning, routing, or termination.
- Copying a source paper's visual style even when the new claim is different.
