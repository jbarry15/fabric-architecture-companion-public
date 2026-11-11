# Anti-Pattern: Bypassing OneLake

## What it looks like

- Data pipelines copying data between ADLS Gen2, Databricks, and other stores without making OneLake the governed Fabric access point
- Multiple ETL pipelines moving the same data to different destinations
- No use of OneLake shortcuts; data physically duplicated across systems

## Why it's a problem

- **Multiple copies of the same data** — Increases storage costs and governance complexity
- **Inconsistency** — Different copies drift out of sync, leading to conflicting reports
- **Increased latency** — Extra copy steps add end-to-end processing time
- **Governance gaps** — No single place to enforce access policies or track lineage

## What to do instead

✅ **OneLake as the single governed namespace** — Route analytics access through OneLake, using it as the unified discovery, security, and consumption layer.

✅ **Use shortcuts** — Instead of copying data from ADLS, Databricks, S3, or other supported stores, create OneLake shortcuts that virtualize access without physical movement.

✅ **Minimize data movement** — If data must be copied (for transformation, serving format, or performance), do it once into OneLake, not repeatedly across systems.

## Severity

🚫 **High** — Undermines the core value proposition of Fabric's unified data layer.

## References

- [OneLake shortcuts](https://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts)
- [OneLake overview](https://learn.microsoft.com/en-us/fabric/onelake/onelake-overview)
