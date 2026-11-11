# Anti-Pattern: BI as the Transformation Layer

## What it looks like

- Complex Power Query (M) transformations doing heavy data cleansing, joins, and business logic
- DAX measures implementing what should be pre-computed aggregations
- Business rules (pricing logic, customer segmentation, KPI definitions) living only in .pbix files

## Why it's a problem

- **Not reusable** — Logic trapped in reports can't be shared across teams or consumed by other tools
- **Inconsistency** — Different reports implement the same business rules differently
- **Poor performance** — Power BI is optimized for analytical queries, not ETL workloads
- **Governance blind spot** — No lineage, version control, or testing on transformation logic
- **Fragile** — Report authors shouldn't need to understand source system schemas

## What to do instead

✅ **Push transformation to Silver/Gold layers** — Use Fabric notebooks, dataflows, or pipelines to implement business logic in the data platform, not in reports.

✅ **Centralized semantic model** — Define measures, hierarchies, and business rules in a shared semantic model that all reports consume.

✅ **Reports should be "thin"** — Reports visualize data; they don't transform it.

## Severity

🚫 **High** — Breaks reusability and consistency across teams.

## References

- [Power BI Semantic Models](https://learn.microsoft.com/en-us/power-bi/connect-data/service-datasets-understand)
- [Medallion Lakehouse Architecture](https://learn.microsoft.com/en-us/fabric/onelake/onelake-medallion-lakehouse-architecture)
