# Anti-Pattern: Skipping Medallion Layers (Raw → BI Directly)

## What it looks like

- Raw source data (JSON, CSV, API responses) consumed directly by Power BI semantic models
- No intermediate transformation or validation layer
- Reports built on untyped, uncleansed data

## Why it's a problem

- **No data quality gate** — Errors, nulls, schema changes in source data propagate directly to business reports
- **No separation of concerns** — Consumers (analysts, reports) are exposed to raw system data
- **Breaks reliability** — Any source schema change instantly breaks downstream reports
- **No reusability** — Each report team cleans/transforms independently, duplicating effort

## What to do instead

✅ **Implement full Medallion architecture:**

| Layer | Purpose | Content |
|-------|---------|---------|
| **Bronze** | Raw ingestion | Source data as-is, append-only, full history |
| **Silver** | Conformed/standardized | Typed, deduplicated, validated, joined |
| **Gold** | Business-ready | Aggregated, modeled (star schema), ready for BI |

✅ **Do not skip the Silver responsibilities** — Data quality, conformity, and governance need a clear enforcement point. That is usually a physical Silver layer, but can be a deliberate logical/virtualized contract for small or transitional cases.

## Severity

🚫 **High** — Undermines data quality and reliability that Medallion is designed to enforce.

## References

- [Medallion Lakehouse Architecture in Fabric](https://learn.microsoft.com/en-us/fabric/onelake/onelake-medallion-lakehouse-architecture)
