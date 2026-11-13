# Synapse to Fabric migration utilities

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Reference implementation |
| Primary workload | Data Warehouse |
| Related workloads | Data Engineering, Azure Synapse Analytics |
| Public starting point | [microsoft/fabric-migration](https://github.com/microsoft/fabric-migration) |

## Scenario

A team needs scripts and utilities to assist migration of Azure Synapse dedicated SQL pools or Spark workloads to Fabric.

## Recommended approach

Use the upstream Data Warehouse or Data Engineering path for the workload being assessed. Start with the corresponding Microsoft Learn migration plan, inventory dependencies, and pilot a small approved scope before adapting scripts.

Maintain one migration plan for schema and code conversion, data access or movement, security, orchestration, validation, and cutover. The same repository covers both SQL and Spark; this is its single catalog card.

## Prerequisites and watch-outs

- This is a collection of migration aids, not a guarantee of automatic, complete, or lossless Synapse estate migration.
- Validate SQL compatibility, Spark runtime and library differences, source-specific integrations, and target access requirements before executing a script.
- Review source and target permissions, credentials, generated files, data handling, and temporary parallel-running costs.
- Preserve a tested recovery path and explicit acceptance criteria. A successful script run does not establish application or reporting parity.

## Validation before use

- Reconcile representative data, calculations, and business queries against the source.
- Test jobs, dependencies, permissions, and expected performance in the target.
- Rehearse incremental loading and cutover, retaining business-owner approval.

## Related guidance

- [Data Warehouse best practices](../../../best-practices/data-warehouse.md)
- [Data Engineering best practices](../../../best-practices/data-engineering.md)
- [Data Engineering and Data Science solutions](../../data-engineering-data-science/README.md)

## Microsoft Learn

- [Plan a dedicated SQL pool migration to Fabric](https://learn.microsoft.com/en-us/fabric/data-warehouse/migration-synapse-dedicated-sql-pool-planning)
- [Migrate Azure Synapse Spark to Fabric](https://learn.microsoft.com/en-us/fabric/data-engineering/migrate-synapse-overview)
