---
id: rabitq
type: entity
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - method
  - ann
  - retrieval
  - rabitq
source_count: 1
sources:
  - raw/sources/papers/rabitq-2024.pdf
related:
  - rabitq-extension-2024
  - approximate-nearest-neighbor-search
  - vector-quantization
  - product-quantization
confidence: medium
---

# RaBitQ

## Profile

RaBitQ is a quantization family for approximate nearest-neighbor search that uses random rotations and theoretically controlled inner-product estimation. In the current vault, it appears through a 2024 paper that extends the method from 1-bit codes to multi-bit-per-dimension ANN quantization.

## Core Mechanism

- Normalize vectors and work through inner-product estimation.
- Use random orthogonal rotations to build a data-oblivious codebook.
- Represent vectors with compact codes and estimate distances from the codes rather than raw vectors.
- In the multi-bit extension, use the first bits for fast pruning and the remaining bits for incremental refinement.

## Why It Is Important

- It is a serious alternative to [Product Quantization](product-quantization.md) in the retrieval branch of this vault.
- It emphasizes strong recall without keeping raw vectors in RAM for reranking.
- It adds a theory-heavy, data-oblivious retrieval method to a cluster that already contains PQ and TurboQuant.

## Caveats

- The current source is ANN-specific and does not show a broader inference-oriented story like [TurboQuant](turboquant.md).
- The vault currently lacks the original RaBitQ paper, so this page is partially reconstructed from the extension paper.

## Related Pages

- [RaBitQ Extension Source Note](../source-notes/rabitq-extension-2024.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
- [Product Quantization](product-quantization.md)
