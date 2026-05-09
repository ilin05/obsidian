# ANNS Figure Drawing Cache

Fast entry point for agents drawing or reviewing ANNS research figures.

## First Read

1. Full guide: [ANNS Research Figure Design Guide](wiki/analyses/anns-research-figure-design-guide.md)
2. Skill: [ANNS Research Figure Drawing Skill](skills/anns-research-figure-drawing/SKILL.md)
3. Main ANNS context: [CACHE.md](CACHE.md)
4. Evaluation rules: [ANN Benchmarking Methodology](wiki/topics/ann-benchmarking-methodology.md)

## Choose The Figure Type

| Need | Start from these references |
|---|---|
| System architecture | [CXL-ANNS p13](wiki/assets/figure-guide/cxl-anns-p13-overview.png), [FusionANNS p8](wiki/assets/figure-guide/fusionanns-p8-architecture-workflow.png), [BANG p4](wiki/assets/figure-guide/bang-p4-schema.png), [GustANN p8](wiki/assets/figure-guide/gustann-p8-architecture.png), [SVFusion p4](wiki/assets/figure-guide/svfusion-p4-overview-index.png) |
| Data placement / layout | [Starling p6](wiki/assets/figure-guide/starling-p6-layout-search.png), [PQ p8](wiki/assets/figure-guide/pq-p8-ivfadc-overview.png), [SPFresh p7](wiki/assets/figure-guide/spfresh-p7-architecture.png) |
| Query workflow / pipeline | [RUMMY p6](wiki/assets/figure-guide/rummy-p6-overview-retrofit.png), [FusionANNS p8](wiki/assets/figure-guide/fusionanns-p8-architecture-workflow.png), [BANG p4](wiki/assets/figure-guide/bang-p4-schema.png), [GustANN p8](wiki/assets/figure-guide/gustann-p8-architecture.png) |
| Graph / algorithm intuition | [Graph survey p7](wiki/assets/figure-guide/graph-survey-p7-pipeline.png), [HNSW p3](wiki/assets/figure-guide/hnsw-p3-hierarchy-neighbor-selection.png), [NSG p7](wiki/assets/figure-guide/nsg-p7-edge-selection.png) |
| Recall-QPS / Pareto evaluation | [ANN-Benchmarks p12](wiki/assets/figure-guide/ann-benchmarks-p12-recall-qps.png), [SPANN p7](wiki/assets/figure-guide/spann-p7-performance.png), [BANG p11](wiki/assets/figure-guide/bang-p11-throughput-recall.png), [Starling p17](wiki/assets/figure-guide/starling-p17-recall-latency-qps.png) |
| Ablation / sensitivity | [SVFusion p10](wiki/assets/figure-guide/svfusion-p10-throughput-latency.png), [FusionANNS p11](wiki/assets/figure-guide/fusionanns-p11-throughput-latency.png), [RUMMY p11](wiki/assets/figure-guide/rummy-p11-performance.png), [GustANN p17](wiki/assets/figure-guide/gustann-p17-search-results.png) |
| Hardware / memory characterization | [CXL memory p5](wiki/assets/figure-guide/cxl-memory-p5-latency-bandwidth.png), [CXL-ANNS p19](wiki/assets/figure-guide/cxl-anns-p19-qps-recall-latency.png), [SmartANNS p11](wiki/assets/figure-guide/smartanns-p11-throughput-recall.png) |

## System Figure Rules

- Show actors, state, tiers, data movement, and online/offline phases.
- Use arrows for actual control/data movement, not decoration.
- Label memory or compute placement: DRAM, GPU HBM, SSD, CXL, RDMA, SmartSSD, CPU.
- If the claim is about reducing traffic, show what moves before and after.
- Keep architecture and experiment figures separate.

## Experiment Figure Rules

- Compare at matched recall when claiming speed.
- Show dataset, metric, units, and baseline names in the figure or caption.
- Use recall-QPS/recall-latency Pareto curves for ANN algorithm comparison.
- Use breakdown/ablation bars for mechanism attribution.
- Use sensitivity curves for parameters, thread count, memory ratio, batch size, or device count.

## Review Loop

Before finalizing a figure, check:

1. Does the figure support one clear paper claim?
2. Are all arrows, colors, abbreviations, axes, and units defined?
3. Would a VLDB/SIGMOD reviewer know the baseline and metric?
4. Is the figure consistent with the implementation and text?
5. Are claims limited to what the figure actually shows?
