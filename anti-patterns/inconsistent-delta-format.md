# Anti-Pattern: Inconsistent Open Table Format for Managed Tables

## What it looks like

- Managed Lakehouse tables mix loose Parquet/CSV folders with Delta tables
- Silver/Gold serving tables are stored as files without Delta log metadata
- BI or downstream jobs query raw file folders directly as if they were governed tables

## Why it's a problem

- **No ACID guarantees for managed tables** — Loose Parquet/CSV folders don't provide Delta transaction semantics
- **No time travel for curated tables** — Can't query historical versions or roll back bad writes
- **Weak schema enforcement** — CSV/JSON files can silently change schema without detection
- **Performance gaps** — Delta table maintenance, data skipping, V-Order, and compaction aren't available for loose file folders
- **Interoperability loss** — Fabric's cross-engine BI and analytics experience works best on governed open table formats

## What to do instead

✅ **Use Delta for managed/conformed/serving tables:**

| Layer | Format | Rationale |
|-------|--------|-----------|
| Bronze | Raw files or Delta | Preserve source fidelity first; convert to Delta when data becomes a managed/queryable table |
| Silver | Delta | Merge/upsert support, time travel for debugging |
| Gold | Delta | Direct Lake compatibility, optimized scans |

✅ **Convert before serving** — Even if sources provide CSV/JSON, convert to Delta before the data becomes a shared Silver or Gold table.

✅ **Enable optimization features:**
- `OPTIMIZE` — Compact small files
- `ZORDER BY` — Co-locate related data for faster scans
- `VACUUM` — Clean up old file versions (after retention period)

## When non-Delta is OK

- External shortcuts to systems that don't support Delta (read-only, landing zone only)
- Temporary staging files that are immediately converted
- Bronze landing zones where preserving the original raw file is required for audit/reprocessing
- Iceberg or other supported open-table interop patterns where that format is deliberate and documented

## Severity

⚠️ **Medium-High** — Misses core platform benefits; should be standardized.

## References

- [OneLake overview](https://learn.microsoft.com/en-us/fabric/onelake/onelake-overview)
- [Delta Lake in Fabric](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-and-delta-tables)
- [Table maintenance](https://learn.microsoft.com/en-us/fabric/data-engineering/delta-optimization-and-v-order)
