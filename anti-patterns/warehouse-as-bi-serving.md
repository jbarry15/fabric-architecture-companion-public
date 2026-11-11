# Anti-Pattern: DirectQuery SQL Endpoint as the Default BI Serving Layer

## What it looks like

- Power BI reports connect to Fabric Warehouse or SQL analytics endpoint via DirectQuery by default
- All interactive BI queries route through the SQL engine even when Direct Lake semantic models are feasible
- The SQL endpoint becomes the front door for every report instead of a governed semantic model

## Why it's a problem

- **Unnecessary abstraction** — Adds a SQL engine hop between BI and the underlying Delta tables when Direct Lake can read them directly
- **Increased latency** — Queries must be parsed, optimized, and executed by the SQL engine vs. loading columnar data into VertiPaq
- **Capacity consumption** — SQL compute CUs are consumed for workloads Direct Lake semantic models can often serve more efficiently
- **Semantic model bypass** — Reports can drift into report-specific logic instead of governed shared measures, relationships, and security

## What to do instead

✅ **Use the Warehouse for governed SQL data marts** — Fabric Warehouse is a valid enterprise data warehouse, SQL-based BI, OLAP, and governed mart engine.

✅ **Serve most Power BI from semantic models** — Build custom semantic models on Warehouse/Lakehouse tables and use Direct Lake when feasible.

✅ **Reserve DirectQuery to the SQL endpoint for explicit constraints** — Use it when T-SQL surface area, source-enforced behavior, or a transitional migration requirement outweighs Direct Lake benefits.

## When SQL Endpoint is OK (Silver pattern)

- Teams migrating from Synapse Dedicated Pools who need T-SQL compatibility during transition
- Ad-hoc data exploration by analysts who prefer SQL
- Cross-lakehouse joins that aren't yet modeled in Gold
- Governed SQL-based BI scenarios where Direct Lake cannot satisfy the requirement yet

## Severity

⚠️ **Medium** — Functional and sometimes required, but suboptimal as the default Power BI serving pattern when Direct Lake semantic models are feasible.

## References

- [Direct Lake overview](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview)
- [Fabric Warehouse overview](https://learn.microsoft.com/en-us/fabric/data-warehouse/data-warehousing)
- [Semantic models on the Warehouse](https://learn.microsoft.com/en-us/fabric/data-warehouse/semantic-models)
