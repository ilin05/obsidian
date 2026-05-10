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
  - lvc
source_count: 2
sources:
  - raw/sources/papers/gorilla-2015.pdf
  - raw/sources/papers/chimp-2022.pdf
related:
  - approximate-nearest-neighbor-search
  - vector-quantization
confidence: high
---

# Lossless Floating Point Compression

## Core Idea

Lossless float compression exploits the IEEE 754 binary representation: when two float values are numerically close, their bitwise XOR produces long runs of leading zeros (shared sign + exponent + high mantissa bits). Instead of storing the full 64-bit value, store only the XOR difference with a reference value. This is a **streaming, single-pass, lossless** technique — unlike general-purpose compressors (Xz, Brotli, LZ4) that require block-level processing, and unlike lossy quantization (PQ, SQ) that discards information.

## Why It Matters for Vector Search

In ANNS systems, vectors are stored in memory/SSD and decompressed on-the-fly during search. General-purpose compression is too slow for query-time decode; lossy quantization loses recall. Lossless float streaming compression sits in a sweet spot:

- **Fast enough for query-time decode** — single-pass bit-stream reading, no block decompression
- **Lossless** — zero recall degradation
- **Exploits data locality** — consecutive graph neighbors have similar vector values
- **Composable with differential encoding** — can chain XOR references across graph edges

LVC (Lossless Vector Compression) builds its coding layer directly on this lineage, extending it from 1D time series to high-dimensional graph-guided differential encoding.

## Algorithm Lineage

### Generation 1: Gorilla (2015)
- XOR with immediately previous value
- 3 cases: identical (1 bit), same block (flag + bits), new block (flag + lead + center + bits)
- Key insight: consecutive time series values are numerically close
- 12× compression on Facebook monitoring data

### Generation 2: Chimp (2022)
- Analyzed 14 diverse float time series — found Gorilla's assumptions don't match data
- Key finding: leading zeros abundant, trailing zeros rare → redesign encoding
- Exponential-decay leading zero encoding (3 bits vs Gorilla's 5)
- Conditional trailing zero handling (only encode when > 6)
- Chimp128: search 128 predecessors for best XOR partner
- ~40% better than Gorilla, competitive with general-purpose

### Generation 3 (LVC's codec layer): DeXOR, Camel, Elf, Gorilla (codec)
- Extend XOR principles to **decimal-aware** encoding (base-10 digit alignment vs binary)
- Per-dimension state machines track leading zero / common prefix patterns
- Graph-guided BFS ordering replaces temporal ordering for reference selection
- Bounded chain length controls decode latency (Section 4.2 of LVC)

## Key Design Dimensions

| Dimension | Gorilla | Chimp | LVC Codecs |
|-----------|---------|-------|------------|
| Reference selection | Temporal predecessor | Best of 128 temporal | Graph neighbor (BFS order) |
| Leading zero encoding | 5-bit linear | 3-bit exp-decay | Dimension-state tracked |
| Trailing zero handling | Always encoded (6-bit) | Conditional (threshold 6) | Decimal-aligned |
| Multi-predecessor | No | Yes (ring buffer + hash) | Yes (BFS chain traversal) |
| Domain | Time series (1D) | Time series (1D) | Vector dimensions (d-D) |

## Open Questions

- How does the leading-zero distribution differ between time-ordered and BFS-ordered vector sequences?
- Can Chimp128's hash-based predecessor lookup be adapted to graph traversal (neighbor ID → vector value)?
- What is the optimal block/chain length for vector data given that decode requires replaying from chain head?
- Does decimal XOR (DeXOR) systematically outperform binary XOR for vector data where many datasets use integer or low-precision decimal values?

## Related Pages

- [Gorilla (2015)](../source-notes/gorilla-2015.md)
- [Chimp (2022)](../source-notes/chimp-2022.md)
- [Approximate Nearest Neighbor Search](approximate-nearest-neighbor-search.md)
- [Vector Quantization](vector-quantization.md)
