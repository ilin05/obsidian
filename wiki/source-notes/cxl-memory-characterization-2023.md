---
id: cxl-memory-characterization-2023
type: source-note
status: active
created: 2026-04-10
updated: 2026-04-10
tags:
  - cxl
  - memory
  - hardware
  - characterization
  - latency
  - bandwidth
source_count: 1
sources:
  - raw/sources/papers/cxl-memory-characterization-2023.pdf
related:
  - cxl-disaggregated-memory-systems
  - disaggregated-memory-vector-search
confidence: high
---

# Demystifying CXL Memory with Genuine CXL-Ready Systems and Devices

**Authors:** Yan Sun, Yifan Yuan, Zeduo Yu, Reese Kuper, Chihun Song, Jinghan Huang, Houxiang Ji, Siddharth Agarwal, Jiaqi Lou, Ipoom Jeong, Ren Wang, Jung Ho Ahn, Tianyin Xu, Nam Sung Kim  
**Affiliations:** UIUC, Intel Labs, Seoul National University  
**Venue:** IEEE/ACM MICRO 2023 (arXiv 2303.15375v4)

## Core Contribution

The first comprehensive study using **true** CXL-ready hardware (4th-gen Intel Xeon Sapphire Rapids + three CXL memory devices from different manufacturers) to characterize CXL latency, bandwidth, cache hierarchy interaction, and application-level impact. Exposes fundamental differences from emulated CXL (DDR5 in remote NUMA node) that invalidate prior emulation-based conclusions.

## Hardware Setup

| Device | IP Type | DRAM Tech | Max BW |
|---|---|---|---|
| DDR5-R (baseline) | — | DDR5-4800 | 38.4 GB/s |
| CXL-A | Hard IP | DDR5-4800 | 38.4 GB/s |
| CXL-B | Hard IP | DDR4-2400 | 40 MB/s LLC |
| CXL-C | Soft IP | DDR3-3200 | 25.6 GB/s |

Server: 2× Intel Xeon 6430 (32 cores, 2.1GHz), 8 DDR5-4800 channels, 1 CXL channel.

## Latency Findings

**Key finding F1:** True CXL memory reduces average memory access latency by ~76% compared to emulated CXL (DDR5 in remote NUMA node).

- Emulated CXL (nemo 1d): ~180ns (2× local DDR5 = ~76ns)
- CXL-A true (1d): ~41ns (comparable to local DDR5 via MLC)
- Idle load latency: CXL-A 29ns, CXL-B ~44ns, DDR5-R 76ns (for 1d)

Mechanisms:
- Full-duplex UPI/PCIe: CXL controller exploits full-duplex to service memory requests in parallel with local CPU cache coherence checks.
- LLC isolation: CXL-evicted cache lines go to LLC before going to CXL device (vs. local DDR which writes directly). This makes CXL accesses benefit from LLC caching.

**Non-temporal store (nt-st) overhead:** CXL-A nt-st latency 25% longer than 1d. Both read and write (nt-st) access required; fence instruction needed. This is important for write-heavy paths.

## Bandwidth Findings

**Key finding O5:** True CXL memory can offer competitive bandwidth efficiency compared to DDR5 for sequential (streaming) workloads.
- CXL-A: 74% of DDR5-R at st, 31%/59% for other types
- CXL-B higher bandwidth efficiency than CXL-A (better-optimized controller)
- CXL-C (FPGA-based Soft IP): lowest bandwidth efficiency (26–20% lower than CXL-B)

**Key finding O3:** Bandwidth is strongly dependent on CXL controller efficiency, not just DRAM technology.

## Cache Hierarchy Interaction

**Key finding O6:** CXL memory interacts with CPU's cache hierarchy differently from local DDR.
- Non-inclusive LLC (Skylake+): L2 evictions go to LLC when data is local DDR; L2 evictions go to CXL device when data is CXL — no intermediate LLC buffer for CXL data.
- In SNC (Sub-NUMA Clustering) mode: CPU cores accessing CXL memory break LLC isolation across SNC nodes, evicting LLC lines that belong to other nodes' local DDR.
- Net effect: CXL access causes 2–4× larger effective LLC capacity for the accessing SNC node, partially compensating for slower access latency.

## Application Impact

**Redis (latency-sensitive, random access):**
- Allocating 100% pages to CXL → 10–105% higher p99 latency than DDR100%.
- Increasing target QPS amplifies latency penalty.
- Redis throughput drops significantly at CXL 100% due to latency sensitivity.

**DLRM embedding reduction (bandwidth-intensive):**
- Allocating 50–63% pages to CXL can improve throughput by up to 88% vs DDR-only (bandwidth expansion benefit).
- CXL acts as bandwidth expander: additional memory channels increase aggregate bandwidth.

**Conclusion:** Latency-sensitive irregular-access workloads (like HNSW traversal) degrade with CXL; bandwidth-intensive sequential workloads benefit.

## CAPTION Dynamic Page Policy

- Monitors CPU counters (L1 miss latency, DDR read latency, IPC) to classify application memory-bandwidth-boundedness.
- Dynamically adjusts percentage of pages allocated to CXL to maximize throughput.
- Improves throughput by up to 24% vs default NUMA page allocation.
- Orthogonal and complementary to TPP (transparent page placement).

## Relevance to CXL ANN Systems

This paper is canonical empirical grounding for CXL-memory system design:

1. **HNSW traversal is latency-sensitive random access** — the Redis result suggests that naively placing random-access ANN state in CXL can cause latency blowup.
2. **CXL latency is not SSD latency** — true CXL at 29–41ns is dramatically lower than SSD (100–200µs), justifying a different algorithm design from DiskANN/SPANN.
3. **Bandwidth expansion** — CXL's bandwidth expansion capability can justify placing cold or verification-stage data in CXL while keeping the hottest metadata in DRAM.
4. **Concrete latency numbers** — 29–41ns for CXL-A gives the quantitative grounding for why bounded rerank depth matters (each rerank fetch ≈ N × 41ns).
5. **SNC mode LLC interaction** — explains why CXL access from multithreaded workers may have different effective cache behavior than single-threaded baseline measurements.

## Open Questions

- How do irregular ANN access patterns interact with the CXL controller's request queuing at high thread counts?
- Does CAPTION-like adaptive page placement have a role in future ANN memory management?
- How much does the 29ns CXL-A latency increase under the multithreaded memory pressure of a saturated HNSW serving workload?
