---
id: dexor-2025
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - float-compression
  - decimal-xor
  - stream-compression
  - time-series
source_count: 1
sources:
  - raw/sources/papers/dexor-2025.pdf
related:
  - lossless-floating-point-compression
  - camel-2024
  - elf-2023
confidence: high
---

# DeXOR: Enabling XOR in Decimal Space for Streaming Lossless Compression of Floating-Point Data

Lv, Li, Yang, Xie, Chen, and Jensen, arXiv 2025 (Zhejiang University / Aalborg University).

## Summary

DeXOR proposes **decimal XOR** — a unified framework that performs XOR-like differencing in decimal space rather than binary. The key observation is that existing methods have a fundamental conflict: Elf's binary erasing wastes bits when two values have different decimal precision (7 erased bits wasted in the running example), and Camel's integer-decimal separation ignores fractional-part smoothness. DeXOR resolves this by extracting the **longest common decimal prefix** between current and reference values (e.g., "64.5" between 64.56 and 64.5487), then scaling the residual suffix to an integer (0.06 → 6). This simultaneously exploits smoothness (via the common prefix) and eliminates redundancy (via suffix scaling). Three supporting optimizations address precision, metadata cost, and extreme cases.

## Key Contributions

- **Decimal XOR procedure**: Instead of binary XOR, find the longest common decimal prefix (LCP) between current and reference values. The prefix is encoded as coordinate metadata; the suffix is scaled to an integer and stored with minimal bits.
- **Scaled truncation with error-tolerant rounding**: Directly computes prefix/suffix from IEEE 754 binary representation without full decimal conversion. Error-tolerant rounding handles edge cases where binary-decimal conversion errors could cause incorrect prefix identification.
- **Bit management strategies**:
  - LCP and suffix encoded with minimal metadata (2-bit case encoding for prefix reuse when consecutive values share the same prefix)
  - Sign consistency check between suffix and prefix to avoid redundant sign bits
  - Fixed-bit unsigned suffix encoding (avoids variable-length overhead when suffix range is small)
- **Exception handler**: For high-precision or non-smooth streams where decimal XOR gains diminish, an exponential-subtraction pipeline runs in parallel. It isolates the floating-point exponent and encodes exponent differences using adaptive-length encoding. Can also function as a standalone compressor for extreme-case datasets.
- **Robustness across conditions**: Maintains competitive compression even where Elf and Camel fail (high-precision datasets, non-smooth data).

## Reported Results

- Compression ratio: **15% higher** than the best competitor across 22 datasets
- Decompression speed: **20% faster** than the best competitor
- Compression speed: competitive with Chimp, faster than Elf
- On low-precision datasets: 30% decompression speed improvement, 20+% compression speed improvement
- Exception handler prevents catastrophic degradation on extreme datasets where other schemes fail entirely

## Significance

DeXOR represents the convergence of the two post-Chimp design branches: Elf's active representation manipulation (erasing) and Camel's decimal-domain awareness (integer/decimal separation). By moving XOR into decimal space, DeXOR achieves what neither could alone — simultaneously exploiting smoothness and eliminating redundancy without the interference that occurs when these are combined in binary. It is the current state-of-the-art streaming float compressor.

## Limits And Open Questions

- Decimal XOR requires the reference and target values to share a meaningful common prefix; on data with no smoothness (random values), it falls back to the exception handler.
- The error-tolerant rounding mechanism adds complexity; formal correctness guarantees under all IEEE 754 rounding modes are not yet proven.
- Current design uses only the immediately preceding value as reference; multi-predecessor search in decimal space is unexplored.
- Interaction with vectorized execution (ALP-style SIMD) is not studied.

## Related Pages

- [Lossless Floating Point Compression](../topics/lossless-floating-point-compression.md)
- [Camel (2024)](camel-2024.md)
- [Elf (2023)](elf-2023.md)
