# Anti-Pattern: Over-Reliance on Import + Scheduled Refresh

## What it looks like

- Large datasets (10GB+) fully imported into Power BI with scheduled refresh every few hours
- Long refresh windows (30+ minutes) blocking other operations
- Stale data between refresh cycles that users complain about

## Why it's a problem

- **Long refresh cycles** — Large imports can take 30-60+ minutes, during which data is stale
- **Memory pressure** — Full imports consume premium capacity memory
- **Concurrency limits** — Refresh slots are limited; many datasets competing for refresh windows
- **Avoids Fabric's intended pattern** — Direct Lake on OneLake tables provides low-latency access through framing/automatic updates without scheduled import refresh overhead

## What to do instead

✅ **Direct Lake mode** — Point semantic models at Delta tables in OneLake. Framing/automatic updates keep the model aligned without importing data into the semantic model, with near-import query performance.

✅ **If Direct Lake isn't feasible** — Use incremental refresh to minimize the refresh window (only process new/changed partitions).

✅ **For real-time needs** — Combine Direct Lake with Real-Time Intelligence (Eventhouse/KQL) for streaming scenarios.

## When Import is still OK (Silver pattern)

- Small datasets with simple refresh requirements and refresh windows that meet the business SLA
- During migration from legacy systems, as a transitional step
- When source data doesn't support Delta format yet

## Severity

⚠️ **Medium** — Acceptable short-term, but should be migrated to Direct Lake for large datasets.

## References

- [Direct Lake overview](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview)
- [How Direct Lake works](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-how-it-works)
- [Incremental refresh](https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-overview)
