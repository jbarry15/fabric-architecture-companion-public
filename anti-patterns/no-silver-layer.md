# Anti-Pattern: No Silver Layer

## What it looks like

- Bronze (raw) data goes directly to Gold (curated) with no intermediate standardization
- Each downstream team writes their own cleansing and joining logic
- No single "source of truth" for conformed dimensions or validated entities

## Why it's a problem

- **Duplicated transformation logic** — Multiple teams independently clean and join the same data
- **No single trusted data foundation** — Different Gold outputs disagree because they started from different interpretations of Bronze
- **Harder to debug** — Without a Silver layer checkpoint, tracing data quality issues back to source is difficult
- **Governance gap** — No layer where data stewards can validate and certify data before business consumption

## What to do instead

✅ **Build a Silver layer** that provides:

- **Typed schemas** — Consistent column names, data types, and nullability
- **Deduplication** — Remove duplicate records from source systems
- **Joins** — Combine related sources into conformed entities
- **Validation** — Apply data quality rules (range checks, referential integrity)
- **Standardization** — Common date formats, currency codes, identifier mappings

✅ **Silver is your governed conformance boundary** that multiple Gold layers can build on. It can be a physical table layer or a deliberate logical/virtualized contract, but the quality and ownership responsibilities still need to exist.

## Severity

🚫 **High** — Prevents creation of a trusted, reusable data foundation.

## References

- [Medallion Lakehouse Architecture in Fabric](https://learn.microsoft.com/en-us/fabric/onelake/onelake-medallion-lakehouse-architecture)
