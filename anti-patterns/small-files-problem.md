# Anti-Pattern: Small Files Problem (Too Many Small Parquet/Delta Files)

## Summary

Allowing Delta tables to accumulate many tiny files instead of letting Fabric maintenance, optimize write, auto compaction, or adaptive target file size keep file layout healthy.

## Why it's a problem

| Impact | Severity |
|--------|----------|
| Exceeds Direct Lake file/row-group guardrails → refresh/query failure or SQL-mode fallback | 🔴 High |
| Excessive metadata overhead → slow file listing and planning | 🔴 High |
| Poor read performance → more I/O operations per query | 🟡 Medium |
| Wasted storage from small file overhead | 🟡 Medium |
| Slow Spark jobs due to excessive task scheduling | 🟡 Medium |

## What this looks like

- Streaming ingestion writing one file per micro-batch
- Frequent small appends (e.g., row-by-row inserts from pipelines)
- Over-partitioning creating many directories with tiny files
- Never running `OPTIMIZE` or table maintenance
- Thousands of Parquet files < 10 MB each in a single table

## What to do instead

1. **Use adaptive target file size** — Let Fabric choose healthy file sizes as tables grow; override only for specific tuning scenarios
2. **Run `OPTIMIZE` regularly** — Compact small files after large ingestion or transformation jobs
3. **Enable auto compaction / optimize write selectively** — Prevent small files at write time for partitioned tables, frequent inserts, and MERGE/UPDATE/DELETE-heavy workloads
4. **Use V-Order for read-heavy BI tables** — Improves compression and Direct Lake performance, but don't force it on every write-heavy staging table
5. **Batch writes appropriately** — Accumulate data before writing rather than row-by-row
6. **Avoid over-partitioning** — Only partition on low-cardinality columns; fewer partitions = larger files per partition
7. **Schedule table maintenance** — Automate `OPTIMIZE` + `VACUUM` as post-ingestion steps or with Lakehouse Maintenance activity
8. **Monitor file counts** — Check table detail/history and Direct Lake guardrails for file/row-group pressure

## Guardrail context (Direct Lake)

| SKU | Max Parquet files per table | Max row groups per table |
|-----|----------------------------|--------------------------|
| F2-F32 | 1,000 | 1,000 |
| F64/FT1/P1-F256/P3 | 5,000 | 5,000 |
| F512/P4 and above | 10,000 | 10,000 |

> When file count exceeds the guardrail, Direct Lake on OneLake refresh fails and the model can't be queried until optimized. Direct Lake on SQL falls back to DirectQuery only when fallback is enabled.

## Example fix

```sql
-- Compact small files with V-Order for read-heavy BI tables
OPTIMIZE gold.fact_sales VORDER;

-- Remove old files outside 7-day retention
VACUUM gold.fact_sales RETAIN 168 HOURS;
```

## References

- [Data engineering best practices](../best-practices/data-engineering.md)
- [Direct Lake best practices](../best-practices/direct-lake.md)
- [Cross-workload table maintenance](https://learn.microsoft.com/en-us/fabric/fundamentals/table-maintenance-optimization)
- [Delta table optimization](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-table-maintenance)
- [Tune file size](https://learn.microsoft.com/en-us/fabric/data-engineering/tune-file-size)
- [V-Order optimization](https://learn.microsoft.com/en-us/fabric/data-engineering/delta-optimization-and-v-order)
