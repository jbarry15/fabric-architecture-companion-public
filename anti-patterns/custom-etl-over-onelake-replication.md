# Anti-Pattern: Hand-Built ETL Over Automatic OneLake Replication

## What it looks like

- Custom Data Factory pipelines (or external ETL) copy operational database data into OneLake on a schedule
- Building bespoke mirroring/CDC logic for sources that already replicate automatically
- Treating a [SQL database in Fabric](../best-practices/databases-in-fabric.md), Cosmos DB in Fabric, or a [mirrored database](../best-practices/mirroring.md) as if it needed a manual copy job to reach the analytical estate

## Why it's a problem

- **Redundant work** — SQL database and Cosmos DB in Fabric **auto-replicate to OneLake in open Delta near real time**; Fabric Mirroring covers many external operational sources, open mirroring scenarios, and selected metadata mirroring. A hand-built pipeline often duplicates a capability the platform already provides.
- **Added latency** — Scheduled batch copies lag the source; the built-in replication is continuous.
- **Extra cost & CU consumption** — Pipeline compute runs you don't need to pay for.
- **Data drift & maintenance** — Custom copies can diverge from source, break on schema change, and become an ongoing operational burden.

## What to do instead

✅ **Use the automatic OneLake replica** — consume the auto-generated Delta tables directly from Spark, Warehouse, notebooks, and [Direct Lake](../best-practices/direct-lake.md). See [SQL DB mirroring/replication](https://learn.microsoft.com/en-us/fabric/database/sql/mirroring-overview) and [Cosmos DB mirror to OneLake](https://learn.microsoft.com/en-us/fabric/database/cosmos-db/mirror-onelake).

✅ **Use Mirroring for other operational sources** — for already-curated or supported external operational sources, [Mirroring](../best-practices/mirroring.md) continuously replicates into OneLake with minimal setup.

✅ **Reserve Copy job/pipelines for what needs them** — raw ingestion from non-replicating sources, transformations, and orchestration. See [Data Factory best practices](../best-practices/data-factory.md).

## When a custom pipeline is OK (Silver pattern)

- The source is **not** a Fabric-native or mirrorable store (e.g., a third-party API or flat-file feed) and genuinely needs a copy job
- You need transformation/schema mapping at ingest that replication doesn't perform
- Transitional state during a migration before mirroring is configured

## Severity

⚠️ **Medium** — Works, but wastes compute and adds latency and maintenance; switch to built-in replication.

## References

- [Databases in Fabric](https://learn.microsoft.com/en-us/fabric/database/)
- [Mirroring in Fabric](https://learn.microsoft.com/en-us/fabric/mirroring/overview)
- [SQL database mirroring / replication](https://learn.microsoft.com/en-us/fabric/database/sql/mirroring-overview)
- [Cosmos DB mirror to OneLake](https://learn.microsoft.com/en-us/fabric/database/cosmos-db/mirror-onelake)
- [Data movement decision guide](https://learn.microsoft.com/en-us/fabric/data-factory/decision-guide-data-movement)
