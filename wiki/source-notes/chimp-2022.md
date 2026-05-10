---
id: chimp-2022
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - time-series
  - float-compression
  - xor-encoding
  - stream-compression
source_count: 1
sources:
  - raw/sources/papers/chimp-2022.pdf
related:
  - lossless-floating-point-compression
  - gorilla-2015
confidence: high
---

# Chimp: Efficient Lossless Floating Point Compression for Time Series Databases

Liakos, Papakonstantinopoulou, and Kotidis, PVLDB 2022 (Athens University of Economics and Business).

## Summary

Chimp re-examines the design assumptions of Gorilla's XOR-based float compression against 14 diverse real-world time series. It identifies two critical mismatches: (1) real float time series XOR values have many leading zeros but **few trailing zeros**, making Gorilla's 6-bit trailing-zero encoding wasteful; (2) identical consecutive values are rare in float data, making Gorilla's single-bit equality flag misallocated. Chimp introduces an improved encoding with exponential-decay leading-zero representation, conditional trailing-zero handling, and a multi-predecessor variant (Chimp128) that searches 128 previous values for the best XOR partner. Chimp achieves ~10% better compression than Gorilla and Chimp128 achieves ~40% better, competitive with general-purpose compressors (Xz, Brotli) while being 48× faster.

## Key Contributions

- **Empirical characterization of float time series XOR properties**: Across 14 diverse datasets, leading zeros are abundant (typically 12–30) while trailing zeros are rare (< 6 in most cases).
- **3-bit exponential-decay leading zero encoding**: Steps at {0, 8, 12, 16, 18, 20, 22, 24} save ~0.95 bits/value vs Gorilla's 5-bit linear encoding. Uses a lossy mapping for odd numbers with error always = 1, compensated by an extra zero bit (overall at least as compact, Theorem 1).
- **Conditional trailing zero encoding (threshold = 6)**: Only encode trailing zero count when > 6 (worth the 6-bit cost). Otherwise write the full non-lead part raw — cheaper on average.
- **Four flag cases instead of three**: Better aligned with actual data distribution where identical values are uncommon.
- **Previous leading-zero reuse**: If current leading zeros = previous leading zeros, skip the 3-bit encoding entirely (write `0` control bit).
- **Chimp128: multi-predecessor search**: Ring buffer (size 128) + 14-bit hash table for O(1) lookup of the most recent value sharing ≥ 14 trailing bits. Only use the matched predecessor when trailing zeros > log₂(128) + log₂(64) = 13 bits; otherwise fall back to immediately previous value.

## Algorithm Details

### Chimp Encoding
```
1. First value: uncompressed (64 bits)
2. XOR with previous value:
   If trailing_zeros > 6:
     → write '0' + (if XOR=0: '0' | else: '1' + lead(3b) + center_bits(6b) + meaningful)
   Else (trailing_zeros ≤ 6):
     → write '1' + (if lead == prev_lead: '0' + meaningful | else: '1' + lead(3b) + meaningful)
```

### Chimp128 Encoding
```
1. Find best predecessor among last 128 values (O(1) via hash: v & 0x3FFF as array index)
2. If trailing_zeros > 13:
     → use matched predecessor, store '0' + predecessor_index(7b) + [lead + center + meaningful]
   Else:
     → fall back to immediately previous value (same as Chimp)
```

## Reported Results

- Chimp: **8.3% better than FPC, 9.6% better than Gorilla** on average across 19 datasets
- Chimp128: **39% better than FPC, 40% better than Gorilla**, competitive with Xz/Brotli
- Compression time: **1.1–1.4× faster than Gorilla** (fewer bits written = less bit-stream I/O)
- Decompression time: slightly faster than Gorilla, both ~30 μs/block (1000 values)
- Chimp128 is 4.5–48× faster than general-purpose algorithms that achieve similar compression (Xz, Brotli)
- On single-precision (32-bit): similar pattern — Chimp64 outperforms Gorilla

## Relevance to LVC

Chimp is the **second-generation XOR codec** that LVC's coding layer directly builds upon. Three specific connections:

1. **Leading-zero analysis**: Chimp proved that float differential residuals are dominated by leading zeros. This directly justifies LVC's per-dimension state machine design — the state tracks leading zero patterns dimension-by-dimension.
2. **Multi-predecessor search (Chimp128)**: The ring buffer + hash lookup for finding the best XOR partner among 128 predecessors is conceptually analogous to LVC's BFS-ordered reference chain selection in the graph. LVC replaces temporal ordering with graph-guided ordering.
3. **Conditional encoding thresholds**: LVC's DeXOR and Camel codecs extend Chimp's conditional approach — instead of a fixed threshold (6 trailing zeros), they use decimal-aware thresholds tied to the dimension's value distribution.

## Limits And Open Questions

- Chimp128's hash-based predecessor lookup (using low 14 bits of value) works well for time series but may degrade for vector data with different value distributions per dimension.
- The ring buffer approach assumes temporal locality; graph-guided BFS ordering in LVC must provide analogous locality guarantees.
- Single-precision variant (32-bit) uses Chimp64 (64 predecessors); scaling to vector dimensions requires per-dimension state management.

## Related Pages

- [Lossless Floating Point Compression](../topics/lossless-floating-point-compression.md)
- [Gorilla (2015)](gorilla-2015.md)
