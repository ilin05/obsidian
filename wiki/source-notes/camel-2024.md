---
id: camel-2024
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - float-compression
  - integer-decimal-separation
  - stream-compression
  - time-series
source_count: 1
sources:
  - raw/sources/papers/camel-2024.pdf
related:
  - lossless-floating-point-compression
  - dexor-2025
  - elf-2023
confidence: high
---

# Camel: Efficient Compression of Floating-Point Time Series

Yao, Chen, Fang, Gao, Jensen, and Li, SIGMOD 2024 (Zhejiang University / Aalborg University).

## Summary

Camel observes that existing XOR-based methods compress the integer and decimal parts of floating-point values as a whole, disregarding their different statistical properties. Analysis of 25 datasets reveals that **95% of consecutive values differ by ≤2 in their integer parts** — the integer component is highly stable and compressible with simple delta encoding. The decimal part, however, has richer variation and benefits from XOR-based techniques. Camel **separates integer and decimal parts** and compresses each with a tailored strategy. For the decimal part, instead of XORing with the previous value (which produces randomly distributed trailing zeros), Camel identifies a **reference value** that yields stable, predictable trailing zero patterns. Camel also supports configurable lossy mode and includes an index for querying compressed data without full decompression.

## Key Contributions

- **Integer-decimal separation**: Decompose each float value into integer (`.int`) and decimal (`.dec`) parts in decimal representation. Compress integer parts with delta encoding (95% of deltas are ≤2). Compress decimal parts with a novel XOR strategy.
- **Decimal XOR with stable trailing zeros**: Find a reference value `v_ref` such that `v_curr.dec ⊕ v_ref` produces a consistent, large number of trailing zeros. For example, `1.7 ⊕ 1.2`, `1.78 ⊕ 1.03`, `1.942 ⊕ 1.067` all produce 49–51 trailing zeros. This stability allows compact fixed-bit encoding of center bits.
- **Lossy mode with high precision**: For values with excessive decimal digits, truncate to a configurable precision (e.g., 4 significant digits → 99.9999% precision). The lossy mode is optional and tunable.
- **Camel Index**: An index structure built over compressed data that supports four query types without full decompression: range queries, value queries, time queries, and segment queries. This addresses a key limitation of prior streaming compressors (Gorilla, Chimp, Elf) that cannot query compressed data efficiently.
- **Comprehensive evaluation**: 11 lossless + 6 lossy methods on 22 public datasets + 3 AliCloud industrial datasets.

## Reported Results

- Compression ratio: outperforms all 11 lossless competitors on most datasets
- Compression speed: faster than Elf and competitive with Chimp (no erasing/restoring overhead)
- Lossy mode at 4-digit precision achieves near-lossless accuracy with 20–40% additional compression
- Camel Index: supports queries on compressed data with index size <5% of compressed data
- Strong performance on both time series and non-time-series data

## Significance

Camel was the first method to explicitly separate integer and decimal components for float compression, exploiting their distinct statistical properties. This design directly inspired DeXOR's decimal XOR, which unifies the integer-decimal separation with common prefix extraction. Camel's index structure is also notable — it is one of very few streaming float compressors to support query operations on compressed data, bridging compression and database indexing.

## Limits And Open Questions

- The reference value for decimal XOR is identified via a simple heuristic; optimal reference selection across the full history is not explored.
- Integer-decimal separation requires knowing the decimal place count, which may not be uniform across a dataset.
- The lossy mode's precision guarantees are empirical rather than formal.
- The Camel Index supports four query types but does not address aggregation queries or joins on compressed data.
- Interaction with multi-predecessor search (Chimp128-style) for the decimal part is not studied.

## Related Pages

- [Lossless Floating Point Compression](../topics/lossless-floating-point-compression.md)
- [DeXOR (2025)](dexor-2025.md)
- [Elf (2023)](elf-2023.md)
