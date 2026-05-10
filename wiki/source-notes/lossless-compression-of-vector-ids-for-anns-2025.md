---
id: lossless-compression-of-vector-ids-for-anns-2025
type: source-note
status: active
created: 2026-05-10
updated: 2026-05-10
tags:
  - vector-compression
  - lossless
  - id-compression
  - ans
  - graph-index
  - ivf
source_count: 1
sources:
  - raw/sources/papers/lossless-compression-of-vector-ids-for-anns-2025.pdf
related:
  - vector-compression
  - approximate-nearest-neighbor-search
confidence: high
---

# Lossless Compression of Vector IDs for Approximate Nearest Neighbor Search

Daniel Severo, Giuseppe Ottaviano, Matthew Muckley, Karen Ullrich, and Matthijs Douze, arXiv 2025 (Meta FAIR).

## Summary

This work addresses a neglected aspect of ANN index compression: auxiliary data such as vector IDs and graph edges, which can dominate storage in IVF and graph-based indexes. The key insight: within IVF lists or graph adjacency lists, the ordering of IDs is semantically irrelevant — this frees log n! bits of information that can be exploited by set/multiset compression. Using Asymmetric Numeral Systems (ANS) and wavelet trees, vector IDs are compressed by up to 7×, reducing total index size by 30% on billion-scale datasets, with no impact on search accuracy or speed.

## Key Contributions

- **First systematic evaluation of lossless ID compression for online ANN search**: Prior work ignored that IDs and edges often dominate storage after vector quantization.
- **ANS-based codecs for IVF and graph indexes**: Designs online-decodable compression schemes using ANS that exploit the order-irrelevance property of ID sets within clusters/lists.
- **Offline graph compression**: For whole-index compression (storage/transmission), exploits graph isomorphism under node relabeling for additional savings.
- **Entropy coding of PQ codes**: As a secondary finding, shows that entropy coding can further compress PQ codes that are sub-optimally quantized (non-uniform code distribution).
- **30% index size reduction on billion-scale**: With zero accuracy or latency impact in the online setting.

## Algorithm Details

### Online ID Compression (IVF lists)
1. Within each IVF cluster, treat the list of vector IDs as a **multiset** — order is irrelevant.
2. Sort IDs and encode deltas using ANS with a Laplacian/geometric distribution model.
3. At search time, decompress the ID list sequentially to reconstruct the cluster contents.

### Graph Edge Compression
1. Adjacency list per node is a set of neighbor IDs.
2. Encode each adjacency list independently using ANS over sorted ID deltas.

### Wavelet Tree Alternative
- Provides O(log N) random access to individual elements in a compressed set, useful when only a subset of edges needs to be decoded.

## Reported Results

- IVF ID compression: up to 7× (from 32-bit to ~4.6 bits per ID)
- Graph edge compression: 2–3× on NSG adjacency lists
- Total index size reduction: ~30% on billion-scale (Deep1B, BIGANN)
- Search speed: no measurable overhead for IVF; minor overhead for graph due to per-edge decoding
- PQ code entropy coding: additional 10–20% on codes from suboptimal quantizers

## Significance

This paper reframes the memory bottleneck in ANN serving: after aggressive vector quantization, the metadata (IDs, edges) becomes the dominant storage cost. The information-theoretic insight that set order carries log n! bits is elegantly applied to practical ANN data structures. It also connects the ANN community to mature entropy coding techniques (ANS) that are standard in data compression but underused in vector databases.

## Limits And Open Questions

- Random access to individual IDs in a compressed list is not supported in the basic scheme; wavelet trees add overhead.
- Compression ratio depends on cluster/list size — small lists benefit less.
- The approach assumes static or batch-rebuilt indexes; incremental updates with compressed IDs is an open problem.
- Graph edge compression during online search adds per-edge decode overhead that grows with compression aggressiveness.

## Related Pages

- [Vector Compression](../topics/vector-compression.md)
- [Approximate Nearest Neighbor Search](../topics/approximate-nearest-neighbor-search.md)
