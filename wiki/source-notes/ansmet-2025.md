---
id: ansmet-2025
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - ann
  - near-memory-processing
  - hardware-software-codesign
source_count: 1
sources:
  - raw/ANSMET- .pdf
related:
  - ansmet
  - approximate-nearest-neighbor-search
confidence: high
---

# ANSMET: Approximate Nearest Neighbor Search with Near-Memory Processing and Hybrid Early Termination

## Summary

ANSMET proposes a hardware-software co-design for ANN search that offloads distance operations to near-memory units and applies hybrid early termination using partial dimensions/bits.

## Key Contributions

- Offloads distance calculation/comparison to near-memory processing units.
- Uses hybrid early termination with conservative lower-bound checks from partially fetched vector data.
- Optimizes layout and host-device coordination for irregular early-termination execution.

## Reported Results

- Reports average speedup from near-memory processing versus general-purpose baseline.
- Reports additional speedup when enabling hybrid early termination on top of near-memory processing.

## Limits And Open Questions

- Requires specialized hardware support and careful host-memory co-design.
- Production applicability depends on memory-system integration cost and software ecosystem maturity.

## Related Pages

- [ANSMET](../entities/ansmet.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)

