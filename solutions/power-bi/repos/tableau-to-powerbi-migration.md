# AI-assisted Tableau to Power BI migration

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Reference implementation |
| Primary workload | Power BI |
| Related workloads | Semantic modeling, Microsoft Fabric |
| Public starting point | [Guust-Franssens/tableau-to-powerbi-migration](https://github.com/Guust-Franssens/tableau-to-powerbi-migration) |

## Scenario

A team wants to assess AI-assisted conversion of Tableau workbooks into reviewable Power BI semantic models and reports.

## Recommended approach

Pilot one approved workbook before considering an estate-wide migration. The community toolkit combines Tableau parsing with Copilot CLI agents for model/report generation and validation.

Review the upstream operator guide and [capabilities and limitations](https://github.com/Guust-Franssens/tableau-to-powerbi-migration/blob/master/docs/capabilities-and-limitations.md). Treat the generated PBIP project as a migration candidate, not an accepted replacement. Prefer reusable, governed semantic models over duplicating business logic per report.

## Prerequisites and watch-outs

- This is an unofficial community project, not a Microsoft or Tableau migration guarantee.
- Verify the current Python, Power BI Desktop, Copilot, plugin, and MCP prerequisites in the upstream preflight guidance.
- Unsupported constructs and changed filter or calculation semantics can produce plausible but incorrect output. A project that opens or renders has not demonstrated numeric or functional parity.
- Confirm permission to process source workbooks, extracts, and screenshots with the chosen AI tools. Public examples do not grant reuse rights to every source asset.

## Validation before use

- Compare figures and calculations under identical filters against the Tableau source.
- Test interactions, security, refresh, and performance with representative users.
- Record manual remediation and obtain business-owner acceptance before replacement.

## Related guidance

- [Semantic modeling](../../../best-practices/semantic-modeling.md)
- [Power BI connectivity decisions](../connectivity-decisions.md)

## Microsoft Learn

- [Create and validate content when migrating to Power BI](https://learn.microsoft.com/en-us/power-bi/guidance/powerbi-migration-create-validate-content)
