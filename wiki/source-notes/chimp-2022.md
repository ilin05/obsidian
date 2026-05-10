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

## Significance

Chimp is the definitive empirical re-examination of XOR-based float compression. By characterizing 14 diverse time series, it identified that Gorilla's core assumptions (many trailing zeros, frequent identical values) do not hold for real float data. Its exponential-decay leading-zero encoding and conditional trailing-zero handling are now standard techniques. Chimp128's hash-based multi-predecessor search demonstrated that looking beyond the immediately previous value yields major gains — a principle later adopted by Elf (via erasing to force trailing zeros) and DeXOR (via decimal common prefix search). Chimp established the speed-size Pareto frontier that subsequent work (Elf, ALP, Camel, DeXOR) competes against.

## Limits And Open Questions

- Chimp128's hash-based predecessor lookup (low 14 bits of value as array index) exploits temporal locality — the effectiveness on non-time-series data where values are not temporally ordered is less characterized.
- The fixed threshold of 6 for trailing-zero encoding is derived empirically from the 14 datasets studied; optimality on unseen data distributions is not guaranteed.
- Uses 128 predecessors as a fixed constant; the trade-off between search window size, hash collision rate, and compression gain is dataset-dependent.
- Single-precision mode (32-bit, Chimp64) reduces the predecessor window to 64; the scaling behavior with precision is not formally analyzed.

## Related Pages

- [Lossless Floating Point Compression](../topics/lossless-floating-point-compression.md)
- [Gorilla (2015)](gorilla-2015.md)
