# Best Practice: Direct Lake Semantic Models

## Overview

Direct Lake is Fabric's recommended connectivity mode for Power BI semantic models over Fabric data. It reads Delta/Parquet tables directly from OneLake into memory — combining import-like query performance with low-latency data updates through framing, without importing data into the semantic model.

## How it works

```
Delta Tables (OneLake) → Direct Lake (framing) → VertiPaq Engine → Reports
```

- **Framing** — A lightweight metadata operation (seconds) that synchronizes the semantic model with the latest Delta table version. No data is copied.
- **Transcoding** — Columns are loaded on-demand into memory from Parquet files when a DAX query needs them.
- **Memory states** — Cold → Semiwarm → Warm → Hot. Performance improves as more data is resident in memory.

## Two Direct Lake modes

| Mode | Source | Fallback | Composite model support |
|------|--------|----------|------------------------|
| **Direct Lake on OneLake** | OneLake Delta tables, including supported shortcuts | No DirectQuery fallback — refresh/query fails if guardrails are exceeded | Yes (can add Import/DirectQuery/Dual tables where supported) |
| **Direct Lake on SQL** | Single lakehouse, warehouse, or SQL analytics endpoint source | Can fall back to DirectQuery via SQL endpoint when fallback is enabled | No within the same model (use composite model extension) |

> **Key difference**: Direct Lake on OneLake never falls back to DirectQuery. If guardrails are exceeded, refresh fails and the model can't be queried until the Delta tables are optimized. Direct Lake on SQL can fall back to DirectQuery when fallback is enabled; monitor for that behavior and disable fallback when predictable performance is more important than continuity.

## When to use Direct Lake

| Scenario | Direct Lake? |
|----------|:---:|
| Gold layer Delta tables in OneLake | ✅ Preferred |
| Star schema dimensional model | ✅ Ideal |
| Large datasets (10GB – 1TB+) | ✅ Excels here |
| Need low-latency freshness | ✅ Automatic updates/framing |
| Warehouse with well-structured tables | ✅ Use Direct Lake on OneLake or SQL |
| Data in external systems (Databricks, ADLS, S3/GCS) | ⚖️ Use shortcuts, mirroring, or ingestion so the model reads governed OneLake data; don't default to external DirectQuery |
| Complex RLS with many rules | ⚖️ Test; use fixed identity |
| Non-materialized SQL views needed | ⚖️ Direct Lake on SQL can use them but falls back; Direct Lake on OneLake needs tables/materialized views |

## Delta table optimization (critical for all workloads)

Direct Lake performance depends heavily on Delta table health. These practices apply whether your source is a **Lakehouse**, **Warehouse**, or **Eventhouse** with Delta output.

### V-Order compression

- **Enable V-Order for read-heavy tables** consumed by Direct Lake, Power BI, and SQL engines; don't apply it blindly to write-heavy Bronze or transient staging tables
- V-Order enables VertiScan to compute results directly on compressed data (skips decompression)
- Streaming compressed data boosts transcoding efficiency during cold-start loading
- In Fabric Spark, enable V-Order for the target session/table when it fits the workload: `spark.conf.set("spark.sql.parquet.vorder.enabled", "true")`
- Fabric Warehouses apply V-Order automatically

### File layout and row groups

| Guideline | Recommendation |
|-----------|---------------|
| Target file size | Prefer adaptive target file size; typical files land in the 128 MB – 1 GB range |
| Row group size | 1M – 16M rows per row group |
| Minimize file count | Fewer large files > many small files |
| Run OPTIMIZE or auto compaction | Compacts small files into healthier sizes |
| Run VACUUM | Removes old file versions to reduce metadata overhead |

### Capacity guardrails by SKU

| SKU | Max Parquet files/table | Max row groups/table | Max rows/table (M) | Max model size on disk |
|-----|:-:|:-:|:-:|:-:|
| F2–F8 | 1,000 | 1,000 | 300 | 10 GB |
| F16 | 1,000 | 1,000 | 300 | 20 GB |
| F32 | 1,000 | 1,000 | 300 | 40 GB |
| F64/FT1/P1 | 5,000 | 5,000 | 1,500 | Unlimited |
| F128/P2 | 5,000 | 5,000 | 3,000 | Unlimited |
| F256/P3 | 5,000 | 5,000 | 6,000 | Unlimited |
| F512/P4 | 10,000 | 10,000 | 12,000 | Unlimited |
| F1024/P5 and F2048+ | 10,000 | 10,000 | 24,000 | Unlimited |

> **If guardrails are exceeded**: Direct Lake on OneLake refresh fails and the model can't be queried until optimized. Direct Lake on SQL falls back to DirectQuery only when fallback is enabled. Model size is evaluated at the model level; most other guardrails are evaluated per query.

### Data type best practices

- Use **integer types** instead of strings where possible
- Avoid storing integers as string columns
- Reduce **column cardinality** (fewer unique values = better compression)
- Remove unused columns from the semantic model

## Security with Direct Lake

| Security type | Direct Lake on OneLake | Direct Lake on SQL |
|--------------|:---:|:---:|
| Semantic model RLS | ✅ (use fixed identity) | ✅ (use fixed identity) |
| Semantic model OLS | ✅ | ✅ |
| SQL endpoint RLS | ❌ Not applicable | ⚠️ Causes DirectQuery fallback |
| SQL endpoint CLS | ❌ Not applicable | ⚠️ May cause errors |

**Best practice**: Use a [fixed identity](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-fixed-identity) cloud connection instead of SSO when implementing RLS. This avoids per-user token overhead and ensures consistent performance.

## Implementation guidelines

1. **Source from Gold layer** — Direct Lake is ideal for the analytics-ready gold layer in a medallion architecture
2. **Star schema is strongly recommended** — Facts + dimensions with defined relationships
3. **Configure automatic updates intentionally** — Semantic model framing can pick up new data without scheduled import refresh, but automatic updates can be disabled or suspended
4. **Monitor for fallback** (SQL mode) — Use refresh history, DMVs, and the Fabric Capacity Metrics app to detect DirectQuery fallback
5. **Disable fallback when possible** — Set Direct Lake behavior to prevent silent degradation; fail fast to surface optimization issues
6. **Size your capacity correctly** — Match guardrails to your largest table's file/row counts
7. **Schedule OPTIMIZE and VACUUM** — Run daily or after large data loads across all workloads (Spark notebooks, warehouse maintenance, pipeline post-processing)

## Cross-workload guidance

### Lakehouse source
- Run `OPTIMIZE` with V-Order after Spark ETL jobs complete for read-heavy tables
- Partition large tables by date but avoid over-partitioning (increases file count)
- Use bin-packing for tables with many small append files

### Warehouse source
- Tables are automatically V-Ordered in Fabric Warehouse
- Use `ALTER TABLE ... REBUILD` periodically for heavily-updated tables
- Prefer tables over views for Direct Lake (views cause fallback in SQL mode)
- CTAS (CREATE TABLE AS SELECT) produces well-optimized files

### Real-Time Intelligence / Eventhouse
- When materializing KQL query results to Delta for Direct Lake, ensure output is optimized
- Schedule periodic compaction of the Delta output

## Anti-patterns to avoid

| Anti-pattern | Impact | Fix |
|---|---|---|
| **Too many small Parquet files** | Exceeds guardrails, slow transcoding | Run OPTIMIZE regularly |
| **Not using V-Order on read-heavy tables** | Slower cold-start loading and poorer compression | Enable V-Order for Direct Lake / Power BI tables |
| **High-cardinality string columns** | Poor compression, excessive memory | Reduce cardinality or split columns |
| **Ignoring DirectQuery fallback on SQL-mode models** | Silent performance degradation when fallback is enabled | Monitor fallback; disable when possible |
| **Using non-materialized SQL views as Direct Lake source** | Falls back in SQL mode or is unsupported in OneLake mode | Use tables or materialized views |
| **Skipping VACUUM** | Stale files bloat metadata and storage | Schedule VACUUM with retention period |
| **Over-partitioning tables** | Thousands of small files per query | Partition only by high-level date; fewer partitions |
| **RLS without fixed identity** | Per-user SSO tokens, inconsistent perf | Configure fixed identity cloud connection |
| **Mixing Import refresh + Direct Lake** | Confusion about freshness guarantees | Use Direct Lake for all large tables; Import only for small reference data |
| **Not testing capacity guardrails** | Production failures when data grows | POC with production-scale data volumes |

## Comparison

| Mode | Data Freshness | Performance | Capacity Cost | Refresh required |
|------|:---:|:---:|:---:|:---:|
| **Direct Lake** | Low-latency via framing / automatic updates | ⭐⭐⭐ | Low | No import refresh (framing only) |
| Import | Stale (refresh cycle) | ⭐⭐⭐ | High (memory + CPU) | Yes |
| DirectQuery | Real-time | ⭐ | Source-dependent | No |

## Related anti-patterns

- [DirectQuery to external systems](../anti-patterns/directquery-to-external.md)
- [Import + scheduled refresh overuse](../anti-patterns/import-refresh-overuse.md)

## References

- [Direct Lake overview](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview)
- [How Direct Lake works](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-how-it-works)
- [Understand Direct Lake storage](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-understand-storage)
- [Direct Lake fixed identity](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-fixed-identity)
- [Cross-workload table maintenance and optimization](https://learn.microsoft.com/en-us/fabric/fundamentals/table-maintenance-optimization)
- [Delta Lake optimization and V-Order](https://learn.microsoft.com/en-us/fabric/data-engineering/delta-optimization-and-v-order)
- [Capacity guardrails](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview#fabric-capacity-requirements)
- [FabricCAT: Modern Power BI Architecture Choices](https://github.com/microsoft/FabricCAT/blob/main/Direct%20Lake_DirectQuery/Modern%20Power%20BI%20Architecture%20Choices.pdf)
