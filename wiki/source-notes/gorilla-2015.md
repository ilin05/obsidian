---
id: gorilla-2015
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
  - raw/sources/papers/gorilla-2015.pdf
related:
  - lossless-floating-point-compression
  - chimp-2022
confidence: high
---

# Gorilla: A Fast, Scalable, In-Memory Time Series Database

Pelkonen et al., VLDB 2015 (Facebook).

## Summary

Gorilla is Facebook's in-memory time series database (TSDB) that functions as a write-through cache for monitoring data. It introduces a streaming compression scheme for double-precision floating point values based on XOR with the previous value, achieving an average of 1.37 bytes per data point (12× reduction from 16 bytes). This compression enables storing 26 hours of monitoring data entirely in memory, reducing query latency by 73× and improving throughput by 14× compared to an HBase-backed TSDB.

## Key Contributions

- **Delta-of-delta timestamp compression**: Encodes timestamp deltas as differences of consecutive deltas. 96% of timestamps compress to a single `0` bit due to regular reporting intervals.
- **XOR-based floating point value compression**: XOR the current value with the previous value — if close, the IEEE 754 representations share sign, exponent, and high mantissa bits, producing long runs of leading and trailing zeros.
- **Variable-length encoding with three cases**:
  - XOR = 0 (identical values): store `0` (1 bit) — 51% of values
  - Meaningful bits within previous block: `10` + meaningful bits (~30%)
  - Otherwise: `11` + 5-bit leading-zeros count + 6-bit meaningful-bits count + raw bits (~19%)
- **In-memory data structures**: Sharded TSmap with per-time-series spin locks, shared-pointer-based paged scans, and tombstone-based deletion.
- **Production deployment**: 2B unique time series, 12M data points/sec, 80-machine clusters, cross-region replication.

## Algorithm Details

### Value Compression (Section 4.1.2)
```
1. First value: stored uncompressed (64 bits)
2. XOR = 0 → write '0' (1 bit)
3. XOR ≠ 0:
   a. If leading_zeros ≥ prev_leading_zeros AND trailing_zeros ≥ prev_trailing_zeros:
      → write '10' + meaningful bits (reuse previous block position)
   b. Else:
      → write '11' + leading_zeros (5 bits) + meaningful_bits_count (6 bits) + meaningful bits
```

### Timestamp Compression (Section 4.1.1)
- Delta-of-delta: D = (t_n − t_{n−1}) − (t_{n−1} − t_{n−2})
- D = 0 → `0` (1 bit)
- D ∈ [−63, 64] → `10` + 7 bits
- D ∈ [−255, 256] → `110` + 9 bits
- D ∈ [−2047, 2048] → `1110` + 12 bits
- Otherwise → `1111` + 32 bits

## Reported Results

- Average 1.37 bytes per data point (timestamp + value combined) — 12× compression
- 51% of values compressed to a single bit (identical to predecessor)
- Block size of 2 hours gives near-optimal compression; larger blocks yield diminishing returns
- 10× storage reduction enables fully in-memory operation

## Significance

Gorilla is the foundational XOR-based streaming float compression scheme. Its design — XOR with the previous value, encode runs of leading/trailing zeros — established the pattern that all subsequent streaming float codecs (Chimp, Elf, ALP, Camel, DeXOR) build upon. It demonstrated that domain-specific streaming compression can outperform general-purpose compressors on speed while achieving comparable or better compression ratios for time series data.

## Limits And Open Questions

- The assumption that consecutive values have similar IEEE 754 encoding holds for slowly-varying time series but breaks for high-frequency or noisy data.
- Chimp (2022) later showed that Gorilla's design over-allocates bits for trailing zero encoding (6 bits specifying trailing-zero count is often wasted) and under-exploits leading zero structure.
- The block-based grouping (2-hour windows) introduces a trade-off between compression ratio and random-access decode cost — decompressing a single value may require decoding from the block header.
- Uses only the immediately preceding value as reference; later work (Chimp128, DeXOR) showed that searching multiple candidates improves compression.

## Related Pages

- [Lossless Floating Point Compression](../topics/lossless-floating-point-compression.md)
- [Chimp (2022)](chimp-2022.md)
