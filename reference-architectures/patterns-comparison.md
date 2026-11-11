# Microsoft Fabric Architecture Patterns Comparison

## Quick reference matrix

| Area | ✅ Gold Standard (Recommended) | ⚖️ Silver (Transitional) | 🚫 Anti-Pattern (Avoid) |
|------|------|------|------|
| **Data Architecture** | Medallion (Bronze → Silver → Gold) | Partial layering (some separation, inconsistent) | Raw → BI directly (no layered refinement) |
| **Data Storage** | OneLake as single data foundation | Mix of OneLake + external storage | Siloed storage across ADLS, Databricks, etc. |
| **Data Access** | OneLake shortcuts (no duplication) | Selective shortcuts with some duplication | Multiple copies + ETL pipelines moving same data |
| **BI Connectivity** | Direct Lake (reads Delta directly) | Composite models (Direct Lake + Import fallback) | DirectQuery to external systems (e.g., Databricks) |
| **Performance Model** | In-memory + Direct Lake caching | Import with scheduled refresh | Per-query source execution (high latency) |
| **Transformation Layer** | Silver layer for standardized, governed data | Some transformations in pipelines + some in BI | Logic embedded in Power BI reports |
| **Semantic Modeling** | Centralized semantic model (star schema) | Multiple datasets with partial reuse | Report-level datasets with duplicated logic |
| **Data Modeling** | Star schema (facts + dimensions) | Mixed modeling approaches | Wide, denormalized "all-in-one" tables |
| **Governance** | Layered governance (Bronze/Silver/Gold separation) | Partial governance policies | No clear ownership or lineage across layers |
| **Data Movement** | Minimized (virtualized via shortcuts) | Some duplication for legacy reasons | Heavy ETL pipelines + multiple copies |
| **Scalability** | Delta + OneLake + decoupled layers | Scaling constrained by refresh cycles | Coupled systems (BI tied to source system) |
| **Format** | Delta everywhere (ACID, time travel) | Mix of Delta + Parquet | CSV/JSON without Delta benefits |
| **Deployment** | Git + CI/CD pipelines | Manual with some automation | Direct-to-production, no version control |

## How to read this

- **✅ Gold Standard** — Target state. Design new implementations here.
- **⚖️ Silver (Transitional)** — Acceptable during migration. Document a path to Gold.
- **🚫 Anti-Pattern** — Avoid. If found in existing systems, prioritize remediation.

## Typical outcomes

| Pattern Level | Performance | Governance | Maintainability | Cost |
|:---:|:---:|:---:|:---:|:---:|
| ✅ Gold | High, predictable | Full lineage, clear ownership | Teams work independently | Optimized |
| ⚖️ Silver | Acceptable | Partial | Some team dependencies | Moderate waste |
| 🚫 Anti-Pattern | Slow, brittle | Opaque | Tightly coupled | Expensive |

## Detailed docs

### Anti-patterns
- [DirectQuery to external systems](../anti-patterns/directquery-to-external.md)
- [Bypassing OneLake](../anti-patterns/bypassing-onelake.md)
- [Skipping Medallion layers](../anti-patterns/skipping-medallion-layers.md)
- [BI as transformation layer](../anti-patterns/bi-as-transformation-layer.md)
- [No Silver layer](../anti-patterns/no-silver-layer.md)
- [Import + scheduled refresh overuse](../anti-patterns/import-refresh-overuse.md)
- [Warehouse as BI serving layer](../anti-patterns/warehouse-as-bi-serving.md)
- [Wide denormalized tables](../anti-patterns/wide-denormalized-tables.md)
- [Tight coupling](../anti-patterns/tight-coupling.md)
- [Inconsistent Delta format](../anti-patterns/inconsistent-delta-format.md)

### Best practices
- [Medallion architecture](../best-practices/medallion-architecture.md)
- [OneLake shortcuts](../best-practices/onelake-shortcuts.md)
- [Direct Lake](../best-practices/direct-lake.md)
- [Semantic modeling](../best-practices/semantic-modeling.md)
- [Governance](../best-practices/governance.md)
- [CI/CD deployment](../best-practices/ci-cd-deployment.md)
