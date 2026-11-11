# Best Practice: Medallion Architecture (Bronze → Silver → Gold)

## Overview

Medallion architecture organizes data into three progressively refined layers. Each layer has a clear purpose, ownership, and quality guarantee.

## Layers

### Bronze — Raw Ingestion

- **Purpose:** Capture source data exactly as received
- **Format:** Raw files or Delta. Preserve source fidelity first; convert to Delta when the data becomes a managed/queryable table.
- **Quality:** No transformations — preserve source fidelity for auditing and reprocessing
- **Ownership:** Data engineering / ingestion team
- **Pattern:** `lakehouse_bronze/Tables/{source_system}/{entity}`

### Silver — Conformed & Standardized

- **Purpose:** Clean, validate, deduplicate, and join data into a trusted foundation
- **Format:** Delta (merge/upsert)
- **Quality:** Typed schemas, null handling, referential integrity, deduplication
- **Ownership:** Data engineering / data stewardship team
- **Pattern:** `lakehouse_silver/Tables/{domain}/{entity}`

Key Silver layer responsibilities:
- Data type standardization (dates, currencies, identifiers)
- Deduplication and record matching
- Cross-source joins (e.g., customer from CRM + ERP)
- Data quality rules and validation
- SCD Type 2 handling where needed

### Gold — Business-Ready / Curated

- **Purpose:** Modeled, aggregated data ready for BI and consumption
- **Format:** Delta (star schema)
- **Quality:** Business rules applied, dimensional model, optimized for queries
- **Ownership:** Analytics engineering / domain teams
- **Pattern:** `lakehouse_gold/Tables/{domain}/{fact_or_dim}_{entity}`

## Key principles

1. **Persist the contract boundaries** — Bronze/Silver/Gold checkpoints should be auditable and independently reusable, but shortcuts or logical layers can avoid unnecessary physical copies when governance and recovery requirements are still met
2. **Layers are independently processable** — Silver can be rebuilt from Bronze without re-ingesting
3. **Clear ownership** — Each layer has a responsible team
4. **Quality increases with each layer** — Bronze (raw) → Silver (trusted) → Gold (optimized)

## References

- [Implement Medallion Lakehouse Architecture in Fabric](https://learn.microsoft.com/en-us/fabric/onelake/onelake-medallion-lakehouse-architecture)
