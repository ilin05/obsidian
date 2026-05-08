# ANNS System Writer Guide

Use after a review/fix plan exists. The job is to convert review findings into conservative, submission-facing ANNS paper edits.

## Read Order

1. [CACHE.md](CACHE.md)
2. Target section
3. `reviews/<section-name>-review.md`
4. `reviews/<section-name>-fix.md`
5. Implementation/design context, if provided

## Edit Policy

Make the smallest local edits that address reviewer risk. Preserve structure, notation, citations, and prose style unless the review identifies them as the problem.

Allowed:

- weaken unsupported claims;
- define terms, state, metadata, decision rules, and workflow;
- clarify graph construction, pruning, routing, termination, or rerank;
- clarify system placement, scheduling, caching, batching, data movement, or concurrency;
- add forward references to evaluation;
- align text with authoritative implementation context;
- add TODO only when evidence is required and unavailable.

Forbidden:

- fake numbers, results, baselines, citations, or experiments;
- broad rewrites for style only;
- adding subsections unless the fix plan requires it;
- claiming superiority without matched-recall evidence;
- treating implementation benchmarking as proof of an abstract algorithm;
- using in-memory ANN-Benchmarks results as proof for SSD/GPU/CXL/RDMA behavior.

## ANNS Claim Rules

- Always tie performance claims to recall/quality, dataset, metric, index parameters, and hardware tier.
- Distinguish algorithmic contribution from implementation optimization and hardware placement.
- For graph methods, state construction source, neighbor selection/pruning, seed acquisition, routing, and termination.
- For quantization methods, state where approximate distances are used and whether exact rerank/refinement is used.
- For systems methods, state what resides in DRAM/GPU/SSD/CXL/RDMA/NDP, what moves per query, and what is cached or batched.
- For dynamic/vector-DB methods, state update semantics, filter selectivity, freshness, and query correctness/recall impact.

## Useful Safe Wording

- "is designed to reduce"
- "targets"
- "prioritizes"
- "we evaluate at matched recall"
- "adds metadata proportional to"
- "the evaluation isolates this effect using"
- "this does not change the base search semantics"

Avoid: "dramatically", "negligible", "optimal", "guarantees", "scales well", "outperforms" unless evidence is in the section or evaluation.

## Output

If editing a section, also write:

`reviews/<section-name>-writer-change-summary.md`

Include modified file, review inputs, changes made, issues addressed, claims weakened/removed, remaining risks, and one next step.
