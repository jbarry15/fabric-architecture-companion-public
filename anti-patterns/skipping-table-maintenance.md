# Anti-Pattern: Skipping Delta Table Maintenance (No VACUUM or OPTIMIZE)

## Summary

Never running or enabling table maintenance on Delta tables, allowing unbounded file accumulation, metadata bloat, and storage waste.

## Why it's a problem

| Impact | Severity |
|--------|----------|
| Unbounded storage growth from orphaned files | 🔴 High |
| Slow file listing and query planning | 🔴 High |
| Exceeds Direct Lake guardrails as files accumulate | 🔴 High |
| Transaction log bloat slows all operations | 🟡 Medium |
| Time travel retains data indefinitely without VACUUM | 🟡 Medium |

## What this looks like

- Delta table with 50,000+ small Parquet files from months of appends
- Storage costs growing linearly even though logical data size is stable
- Queries getting progressively slower over weeks/months
- `DESCRIBE HISTORY` showing thousands of versions with no cleanup
- Direct Lake models failing because file count exceeds capacity guardrails

## What to do instead

### Regular maintenance schedule

| Operation | Frequency | Purpose |
|-----------|-----------|---------|
| `OPTIMIZE` / auto compaction | After large ingestion batches or when fragmentation is detected | Compact small files to healthy sizes |
| `OPTIMIZE ... VORDER` | On read-heavy Silver/Gold tables consumed by Power BI | Apply V-Order for Direct Lake performance |
| `VACUUM` | Daily or weekly (outside retention window) | Remove orphaned files to reclaim storage |
| `OPTIMIZE ... ZORDER BY (col)` | On frequently filtered columns | Co-locate data for faster predicate evaluation |

### Implementation pattern

```python
# Post-ingestion maintenance notebook
from delta.tables import DeltaTable

tables_to_maintain = ["gold.fact_sales", "gold.fact_orders", "gold.dim_customer"]

for table in tables_to_maintain:
    # Compact small files with V-Order
    spark.sql(f"OPTIMIZE {table} VORDER")
    # Remove old files (7-day retention)
    spark.sql(f"VACUUM {table} RETAIN 168 HOURS")
```

### Scheduling options

| Method | When to use |
|--------|-------------|
| Pipeline activity after ingestion | Maintenance tied to data refresh |
| Scheduled notebook | Regular cadence (nightly, weekly) |
| Lakehouse Maintenance activity | Pipeline-driven Optimize/Vacuum after ingestion or on a schedule |
| Auto compaction / optimize write | Writer-side prevention for frequent inserts, partitioned tables, and merge-heavy workloads |

## Key considerations

- **VACUUM retention**: Default is 7 days. Don't set below this unless you're certain no long-running queries need the old files
- **OPTIMIZE frequency**: Balance between too frequent (wasted compute) and too rare (file count grows)
- **Adaptive target file size**: Prefer Fabric-managed adaptive sizing before hard-coding `delta.targetFileSize`
- **V-Order**: Use on read-heavy tables consumed by Direct Lake / Power BI; expect write overhead
- **Z-ORDER**: Best on columns with high selectivity used in WHERE/filter clauses
- **Warehouse/mirroring**: Some table layout and maintenance is system-managed; don't apply Lakehouse-only maintenance guidance blindly to every Fabric item

## References

- [Data engineering best practices](../best-practices/data-engineering.md)
- [Direct Lake best practices](../best-practices/direct-lake.md)
- [Delta table optimization](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-table-maintenance)
- [Cross-workload table maintenance](https://learn.microsoft.com/en-us/fabric/fundamentals/table-maintenance-optimization)
- [Lakehouse Maintenance activity](https://learn.microsoft.com/en-us/fabric/data-factory/lakehouse-maintenance-activity)
