# Anti-Pattern: DirectQuery to External Systems as the Default BI Pattern

## What it looks like

Power BI semantic models defaulting to DirectQuery connections against external systems like Databricks SQL warehouses, Synapse Dedicated Pools, operational databases, or other non-OneLake sources even when the data could be governed and served through OneLake.

## Why it's a problem

- **Per-query round trips** — Every report interaction hits the external source, creating unpredictable latency
- **Poor scale** — Concurrent users multiply source load; BI performance becomes coupled to source system availability
- **Bypasses Fabric's optimized serving path** — For governed Fabric analytics, the preferred path is OneLake tables with Direct Lake semantic models
- **Cost amplification** — Each query consumes compute on the external system (often billed per-query or per-CU)

## What to do instead

✅ **Use Direct Lake where feasible** — Land, mirror, or shortcut data into OneLake as governed tables, then point semantic models at them using Direct Lake mode. This provides import-like performance without importing data into the model.

✅ **Use the right OneLake path** — Use Mirroring for supported operational databases, shortcuts for no-copy access, and Data Factory/Spark when data needs transformation or format conversion.

✅ **If Direct Lake isn't feasible yet** — Use Import with incremental refresh, aggregations, or carefully tuned DirectQuery as a transitional pattern, but document why the exception exists.

## When DirectQuery is still valid (Silver pattern)

- The source must enforce security, sovereignty, or server-side business rules that can't be replicated into Fabric yet
- Data must stay external for regulatory or contractual reasons
- The source is specifically optimized for high-concurrency analytical DirectQuery
- The scenario is real-time/federated exploration and not a governed enterprise BI serving layer

## Severity

⚠️ **Medium-High** — Valid for specific constraints, but risky as the default Fabric BI architecture.

## References

- [Direct Lake overview](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview)
- [DirectQuery overview](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-directquery-about)
- [DirectQuery model guidance](https://learn.microsoft.com/en-us/power-bi/guidance/directquery-model-guidance)
- [Fabric Well-Architected: Performance Efficiency](https://learn.microsoft.com/en-us/azure/well-architected/microsoft-fabric/)
