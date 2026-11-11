# Anti-Pattern: Tight Coupling of Ingestion, Transformation, and Consumption

## What it looks like

- A single pipeline that ingests data from source, transforms it, and writes directly to a BI-ready format in one step
- No intermediate storage between stages
- Changing transformation logic requires rerunning ingestion
- BI availability depends on the entire pipeline succeeding end-to-end

## Why it's a problem

- **No independent scalability** — Can't scale ingestion separately from transformation
- **Blast radius** — A failure in transformation blocks both ingestion and consumption
- **Team bottlenecks** — One team owns the monolithic pipeline; other teams can't independently iterate
- **Reprocessing waste** — Fixing a transformation bug requires re-ingesting from source
- **Testing difficulty** — Can't test transformation logic without triggering ingestion

## What to do instead

✅ **Decouple into Medallion layers with clear boundaries:**

```
Source → [Ingestion] → Bronze → [Transformation] → Silver → [Curation] → Gold → [BI]
```

Each boundary is:
- **Independently deployable** — Teams own their layer
- **Independently testable** — Silver transformations can be tested on existing Bronze data
- **Fault-isolated** — Ingestion failure doesn't break existing Silver/Gold data
- **Independently scalable** — Heavy transformation can run on larger compute without affecting ingestion

✅ **Use Delta checkpoints** — Each layer persists as Delta tables, providing rollback and time-travel capabilities.

## Severity

🚫 **High** — Limits scalability and team independence; violates separation principles.

## References

- [Medallion Lakehouse Architecture](https://learn.microsoft.com/en-us/fabric/onelake/onelake-medallion-lakehouse-architecture)
- [Fabric Well-Architected: Operational Excellence](https://learn.microsoft.com/en-us/azure/well-architected/microsoft-fabric/)
