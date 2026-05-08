---
id: rummy-2024
type: source-note
status: active
created: 2026-05-08
updated: 2026-05-08
tags:
  - ann
  - vector-search
  - billion-scale
  - gpu
  - host-memory
source_count: 1
sources:
  - raw/sources/papers/rummy-2024.pdf
related:
  - rummy
  - bang
  - gustann
  - fusionanns
  - faiss
  - approximate-nearest-neighbor-search
  - second-tier-memory-for-vector-search
confidence: high
---

# Fast Vector Query Processing for Large Datasets Beyond GPU Memory with Reordered Pipelining

## Bibliographic Note

Zili Zhang, Fangyue Liu, Gang Huang, Xuanzhe Liu, and Xin Jin. 2024. *Fast Vector Query Processing for Large Datasets Beyond GPU Memory with Reordered Pipelining*. NSDI 2024.

Primary page: <https://www.usenix.org/conference/nsdi24/presentation/zhang-zili-pipelining>.

The system name is [RUMMY](../entities/rummy.md).

## Core Problem

RUMMY targets vector query processing when the dataset is too large for GPU memory. The paper argues that GPUs are attractive for vector operations, but billion-scale datasets have footprints of hundreds of GB, exceeding even A100/H100 memory. Existing CPU solutions remain common because naive GPU memory extension either rotates data sequentially or relies on CUDA unified memory page faults.

The paper positions the bottleneck as a host-memory-to-GPU scheduling problem: if the same clusters are repeatedly transferred for different queries in a batch, and if GPU kernels are launched with imbalanced groups, the GPU cannot hide PCIe transfer latency or fully use SMs.

## Design

RUMMY is a query-processing system built on IVF rather than a new ANN index.

- **Cluster-based retrofitting:** convert query-cluster work into a per-cluster execution plan so each involved cluster is transferred at least once, rather than redundantly per query.
- **Dynamic kernel padding with cluster balancing:** split clusters offline into balanced clusters and fill idle GPU SMs online with padding work to reduce spatial and temporal underutilization.
- **Query-aware reordering and grouping:** choose the runtime order and grouping of clusters to overlap large computation with small transfers while avoiding excessive pipeline overhead.
- **GPU memory management:** preallocate and manage GPU memory for vector query batches to reduce fragmentation and cache misses.

## Evaluation Facts

- Venue setup: NSDI 2024; implementation built on FAISS IVF.
- RUMMY evaluates on multiple billion-scale datasets and uses one NVIDIA A100 GPU for the headline GPU comparison.
- Compared with GPU IVF plus CUDA unified memory, RUMMY reports up to 135x higher performance.
- Compared with CPU IVF on 64 vCPUs, RUMMY reports 2.4x-23.1x higher performance.
- The paper reports 26.7x-37.7x better per-dollar performance than the CPU baseline under large batch size.
- RUMMY is close to an ideal lower bound that assumes maximum overlap between host-to-GPU transmission and GPU computation.

## Relevance To Memory-Tiered ANN

RUMMY is useful because it isolates a recurring pattern across out-of-memory ANN systems: the index may be conventional, but the system must schedule candidate movement and computation together.

It is most directly comparable to [BANG](../entities/bang.md), [GustANN](../entities/gustann.md), and [FusionANNS](../entities/fusionanns.md). The distinction is that RUMMY starts from IVF and host-memory extension, while the SSD+GPU systems focus on graph traversal, raw-vector I/O, or SSD bandwidth.

## Follow-up Questions

- Which workloads can use RUMMY's batching assumptions without violating per-query latency targets?
- Can cluster-based retrofitting transfer to other partitioned indexes beyond IVF?
- When comparing against SSD+GPU systems, should RUMMY be treated as a host-memory GPU baseline or a general pipeline-scheduling baseline?
