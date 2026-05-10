---
id: lossless-floating-point-compression
type: topic
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - float-compression
  - xor-encoding
  - stream-compression
  - time-series
  - decimal-encoding
source_count: 7
sources:
  - raw/sources/papers/gorilla-2015.pdf
  - raw/sources/papers/chimp-2022.pdf
  - raw/sources/papers/elf-2023.pdf
  - raw/sources/papers/alp-2023.pdf
  - raw/sources/papers/camel-2024.pdf
  - raw/sources/papers/dexor-2025.pdf
  - raw/sources/papers/lep-2024.pdf
related:
  - approximate-nearest-neighbor-search
  - vector-compression
confidence: high
---

# Lossless Floating Point Compression

## Core Idea

Lossless float compression exploits the IEEE 754 binary representation: when two float values are numerically close, their bitwise XOR produces long runs of leading zeros (shared sign + exponent + high mantissa bits). Instead of storing the full 64-bit value, store only the XOR difference with a reference value. This yields a **streaming, single-pass, lossless** technique — unlike general-purpose compressors (Xz, Brotli, LZ4) requiring block-level processing, and unlike lossy quantization (PQ, SQ) that discards information. The research question driving this field is: given a stream of IEEE 754 doubles, how to select the reference value and encode the XOR result to minimize bits per value while maintaining streaming throughput.

## Algorithm Lineage

### Phase 1: Binary XOR (2015–2022)

**Gorilla (2015)** established the paradigm: XOR the current value with the immediately previous one, encode runs of leading and trailing zeros. Three cases — identical value (1 bit), same block as previous (flag + bits), new block (flag + lead count + center count + bits). Achieved 1.37 bytes/point (12×) on Facebook monitoring data. Widely adopted (InfluxDB, Prometheus, TimescaleDB).

**Chimp (2022)** empirically re-examined Gorilla's assumptions. Key findings: (1) real float time series XOR values have abundant leading zeros but **few trailing zeros** — Gorilla's 6-bit trailing-zero encoding is wasteful in most cases; (2) identical consecutive values are uncommon in float data — the 1-bit equality flag is misallocated. Chimp introduced: exponential-decay leading-zero encoding (3 bits with steps {0,8,12,16,18,20,22,24}), conditional trailing-zero handling (only encode count when >6), and Chimp128 — hash-based O(1) lookup of the best XOR partner among 128 predecessors. Achieved ~40% better compression than Gorilla, competitive with general-purpose compressors while being 48× faster.

### Phase 2: Active Representation Manipulation (2023–2024)

**Elf (2023)** observed that even Chimp128 leaves ~60% of XOR results with ≤5 trailing zeros. Solution: **erase** (zero out) trailing mantissa bits of the current value before XORing — this forces the XOR result to have many trailing zeros. A mathematical framework determines the erasable bit count in O(1) and guarantees lossless recovery via bounded error addition during decompression. Achieved 12.4% better than Chimp128, 43.9% better than Gorilla. Trade-off: erasing/restoring adds ~2–3× compute overhead vs Chimp.

**ALP (2023)** broke from the XOR paradigm entirely. Key insight from 30-dataset analysis: many float values were originally decimals (monetary, coordinates, temperatures) and can be **exactly encoded as scaled integers**. ALP is adaptive per 1024-value vector: if a vector passes decimal detection, use enhanced PseudoDecimals (multiply by common scale factor, bit-pack integers); otherwise, compress only the front bits (sign + exponent + high fraction). Vectorized design (SIMD, auto-vectorized C++) achieves 10–100× faster [de]compression than Chimp/Elf. Not streaming — requires batch vectors.

**Camel (2024)** observed that **integer and decimal parts have different statistical properties**: 95% of integer-part deltas are ≤2 (trivially delta-encodable), while decimal parts benefit from XOR. Camel separates the two components and compresses each with a tailored strategy. For the decimal part, it identifies a reference value producing stable trailing zero patterns (e.g., matching decimal scales so XOR consistently yields 49–51 trailing zeros). Also introduced a query index over compressed data (range, value, time, segment queries). Outperformed 11 lossless + 6 lossy methods across 22 public + 3 industrial datasets.

### Phase 3: Decimal-Space Unification (2025)

**DeXOR (2025)** unified the two post-Chimp branches — Elf's active manipulation and Camel's decimal awareness — via **decimal XOR**. Rather than binary XOR or integer/decimal separation, DeXOR extracts the **longest common decimal prefix** between reference and target (e.g., "64.5" between 64.56 and 64.5487), then scales the residual suffix to an integer (0.06 → 6). This simultaneously exploits smoothness (common prefix) and eliminates redundancy (scaled suffix). Supporting mechanisms: scaled truncation with error-tolerant rounding, 2-bit case encoding for prefix reuse, and an exception handler (exponential subtraction) for extreme cases. Achieved 15% better compression and 20% faster decompression than all prior streaming codecs.

## Key Design Dimensions

| Technique | Reference | Domain | Leading Zero Handling | Trailing Zero Handling | Throughput |
|-----------|-----------|--------|----------------------|----------------------|------------|
| Gorilla | Immediate predecessor | Binary XOR | 5-bit linear | Always encode (6-bit) | Streaming |
| Chimp | Immediate predecessor | Binary XOR | 3-bit exp-decay | Encode if >6 | Streaming |
| Chimp128 | Best of 128 predecessors | Binary XOR | 3-bit exp-decay | Encode if >13 | Streaming |
| Elf | Immediate predecessor | Binary XOR + Erasure | 5-bit (post-erase) | Post-erase: many (>40) | Streaming (slower) |
| ALP | None (self-contained) | Decimal→Int or Front-bits | N/A (columnar) | N/A (columnar) | Vectorized (fastest) |
| Camel | Integer part: delta; Decimal: found ref | Decimal separation + XOR | Per-component | Stable pattern (>49) | Streaming |
| DeXOR | Immediate predecessor | Decimal XOR (LCP) | Prefix metadata (2-bit) | Suffix scaling | Streaming |

## Cross-Cutting Themes

- **Reference selection matters**: The choice of reference value (immediate predecessor, best of N, or found reference with stable trailing-zero properties) has first-order impact on compression ratio. Chimp128 demonstrated this in binary; Camel and DeXOR extended it to decimal.
- **Trailing zeros are the scarce resource**: Leading zeros come naturally from sign/exponent similarity. Trailing zeros are rare and must be actively created (Elf's erasing) or designed for (Camel's reference matching, DeXOR's suffix scaling).
- **Decimal vs binary domain**: The shift from binary XOR (Gorilla, Chimp, Elf) to decimal-space operations (Camel, DeXOR) mirrors the observation that real-world float data often has decimal semantics (monetary values, measurements with fixed precision).
- **Streaming vs vectorized**: The streaming assumption (one value at a time, O(1) state) is standard in time series databases. ALP showed that relaxing this for vector-at-a-time columnar systems yields dramatic speed gains.

## Open Questions

- Can multi-predecessor search (Chimp128-style) be combined with decimal XOR (DeXOR) for additional gains?
- What is the theoretical bound on lossless float compression given a single reference value?
- How do these techniques perform when the data is not a 1D time series but a high-dimensional vector organized along a graph traversal order?
- Is there a unified framework that subsumes erasing (Elf), decimal separation (Camel), and decimal XOR (DeXOR) under a single optimal prefix-extraction model?
- Can ALP's vectorized approach be extended to streaming scenarios via micro-batching?
- How do these float-level encoding techniques transfer to vector embedding compression? LEP (2024) takes a first step by applying ALP per-dimension to embedding matrices, but the broader question of adapting streaming float codecs to multi-dimensional vector data remains open.

## Related Pages

- [Gorilla (2015)](../source-notes/gorilla-2015.md)
- [Chimp (2022)](../source-notes/chimp-2022.md)
- [Elf (2023)](../source-notes/elf-2023.md)
- [ALP (2023)](../source-notes/alp-2023.md)
- [Camel (2024)](../source-notes/camel-2024.md)
- [DeXOR (2025)](../source-notes/dexor-2025.md)
- [LEP (2024)](../source-notes/lep-2024.md)
- [Approximate Nearest Neighbor Search](approximate-nearest-neighbor-search.md)
- [Vector Compression](vector-compression.md)
