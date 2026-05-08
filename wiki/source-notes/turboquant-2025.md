---
id: turboquant-2025
type: source-note
status: active
created: 2026-04-08
updated: 2026-04-08
tags:
  - vector-quantization
  - kv-cache
  - retrieval
  - turboquant
source_count: 1
sources:
  - raw/TurboQuant.pdf
related:
  - turboquant
  - qjl
  - vector-quantization
  - kv-cache-quantization
  - approximate-nearest-neighbor-search
confidence: high
---

# TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate

## Summary

TurboQuant presents a more general vector quantization framework than QJL. Its first stage aims for near-optimal MSE distortion via random rotation plus per-coordinate optimal scalar quantization. Its second stage uses a 1-bit QJL transform on the residual to recover an unbiased inner-product estimator.

## Key Contributions

- Positions vector quantization as a general primitive for both KV-cache compression and nearest-neighbor retrieval.
- Claims near-optimal distortion rates across bit-widths and dimensions up to a small constant factor from information-theoretic lower bounds.
- Uses a two-stage design: MSE-oriented quantization first, then QJL on the residual for unbiased inner products.
- Emphasizes online, data-oblivious operation and accelerator-friendliness instead of learned offline codebooks.

## Reported Results

- For KV-cache quantization, the paper claims quality neutrality at 3.5 bits per channel and only marginal degradation at 2.5 bits per channel.
- For retrieval, it claims better recall than existing product quantization techniques while making indexing time close to zero.
- The paper explicitly frames itself as addressing the limits of both slow offline methods and suboptimal online methods.

## Relationship To Other Sources

- Relative to [QJL](../entities/qjl.md), TurboQuant is broader and uses QJL as a residual inner-product quantizer instead of making QJL the whole method.
- Relative to [Product Quantization](../entities/product-quantization.md), TurboQuant tries to replace learned codebooks with an online data-oblivious pipeline while staying competitive on retrieval tasks.
- It is the clearest synthesis paper in the current cluster because it ties together theory, KV-cache compression, and ANN retrieval.

## Limits And Open Questions

- The theoretical claims are ambitious; this source alone does not show how robust the constant-factor story is across real workloads outside the reported experiments.
- It is not yet obvious whether the best production choice is TurboQuant everywhere or a split strategy: TurboQuant/QJL for online systems and tuned PQ for stable ANN indexes.
- The practical cost of the random rotation stage deserves closer benchmarking in high-throughput deployments.

## Related Pages

- [TurboQuant](../entities/turboquant.md)
- [QJL](../entities/qjl.md)
- [Vector Quantization](../topics/vector-quantization.md)
- [KV Cache Quantization](../topics/kv-cache-quantization.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
