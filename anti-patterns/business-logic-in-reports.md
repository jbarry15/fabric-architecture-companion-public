# Anti-Pattern: Business Logic in Reports Instead of the Semantic Model

## Summary

Implementing business calculations, KPIs, and transformation logic in individual Power BI report visuals or DAX queries at the report level rather than centralizing them in the semantic model.

## Why it's a problem

| Impact | Severity |
|--------|----------|
| Inconsistent metrics across reports — same KPI shows different numbers | 🔴 High |
| Logic duplicated in every report that needs the calculation | 🔴 High |
| Can't enforce business rules centrally | 🟡 Medium |
| Report performance degrades (complex visual-level calculations) | 🟡 Medium |
| Harder to audit and validate business definitions | 🟡 Medium |

## What this looks like

- Revenue calculated differently in the Finance report vs. the Sales report
- Complex DAX measures defined at the visual level rather than in the model
- Report-level calculated columns for transformations that should be in Silver/Gold layers
- Every report author reimplementing the same discount/tax/margin logic
- No "single source of truth" for metric definitions

## What to do instead

### Layer responsibilities

| Layer | Owns |
|-------|------|
| **Gold layer (Lakehouse/Warehouse)** | Data transformations, joins, business keys, deduplication |
| **Semantic model** | Business measures (DAX), KPI definitions, hierarchies, display formatting |
| **Reports** | Visualization, layout, filtering, bookmarks — NOT business logic |

### Best practices

1. **Define all measures in the semantic model** — Use explicit DAX measures, not implicit aggregations
2. **Create a measures table** — Group business KPIs in a dedicated display folder
3. **Use calculation groups** — For variations (YTD, MTD, vs. prior year) of the same base measure
4. **Document measure definitions** — Use descriptions in the model for self-service users
5. **Certify the semantic model** — Mark it as the trusted source; discourage report-level logic
6. **Use composite models sparingly** — Only when the central model truly can't serve the need

## Decision framework

| Question | If yes → |
|----------|----------|
| Will multiple reports need this calculation? | Put it in the semantic model |
| Is this a business-defined KPI (revenue, margin, NPS)? | Put it in the semantic model |
| Is this a one-off exploratory calculation? | Acceptable in the report (temporarily) |
| Does it involve data transformation (joins, dedup)? | Put it in the Gold layer, not the model |

## References

- [Semantic modeling best practices](../best-practices/semantic-modeling.md)
- [Power BI guidance: Star schema](https://learn.microsoft.com/en-us/power-bi/guidance/star-schema)
- [DAX best practices](https://learn.microsoft.com/en-us/power-bi/guidance/)
