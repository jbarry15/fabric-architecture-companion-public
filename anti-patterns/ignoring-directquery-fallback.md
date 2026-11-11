# Anti-Pattern: Ignoring DirectQuery Fallback in Direct Lake

## Summary

Not monitoring or testing whether Direct Lake on SQL models are falling back to DirectQuery mode, or whether Direct Lake on OneLake models are failing refresh/query because guardrails were exceeded.

## Why it's a problem

| Impact | Severity |
|--------|----------|
| 10–100× slower query performance without warning | 🔴 High |
| Users experience intermittent slowness they can't explain | 🔴 High |
| Capacity consumed by SQL endpoint queries instead of in-memory reads when fallback is enabled | 🟡 Medium |
| May indicate underlying data architecture issues (too many files, missing V-Order) | 🟡 Medium |

## What this looks like

- Reports that are fast some days and slow others because SQL-mode models crossed a fallback condition
- No alerts or monitoring on fallback events, refresh warnings, or Direct Lake failures
- Assuming "Direct Lake just works" without validating guardrails
- Using SQL views as Direct Lake on SQL data sources without understanding fallback behavior
- Tables exceeding file count or row group limits for the capacity SKU

## Understanding fallback behavior

| Direct Lake mode | Fallback behavior |
|------------------|-------------------|
| **On OneLake** (OneLake Delta tables/shortcuts) | **No DirectQuery fallback** — refresh fails and the model can't be queried if guardrails are exceeded |
| **On SQL** (Warehouse/SQL analytics endpoint) | Can fall back to DirectQuery when fallback is enabled; refresh succeeds with a warning and queries run slower |

> This makes monitoring critical. Use Direct Lake on OneLake where feasible when you want fail-fast behavior, and configure Direct Lake behavior on SQL-mode models so fallback is an explicit choice.

## What to do instead

1. **Monitor fallback and failures** — Review semantic model refresh history, Fabric Capacity Metrics, DMVs, and Direct Lake diagnostics for fallback or storage failures
2. **Test against guardrails** — Know your SKU limits and validate table file counts stay under them
3. **Set up capacity alerts** — Monitor for DirectQuery activity on models that should be Direct Lake only
4. **Avoid non-materialized SQL views as a default source** — Use Delta tables or materialized views when Direct Lake performance is required
5. **Maintain table health** — Use OPTIMIZE, auto compaction, and sensible file sizing to keep file counts within guardrails
6. **Plan capacity SKU for data volume** — Size your F-SKU based on largest table's file/row-group count

## Guardrail quick reference

| SKU | Max rows per table | Max Parquet files | Max row groups |
|-----|-------------------|-------------------|----------------|
| F2-F8 | 300 M | 1,000 | 1,000 |
| F16 | 300 M | 1,000 | 1,000 |
| F32 | 300 M | 1,000 | 1,000 |
| F64/FT1/P1 | 1.5 B | 5,000 | 5,000 |
| F128/P2 | 3 B | 5,000 | 5,000 |
| F256/P3 | 6 B | 5,000 | 5,000 |
| F512/P4 | 12 B | 10,000 | 10,000 |
| F1024/P5 and F2048+ | 24 B | 10,000 | 10,000 |

## References

- [Direct Lake best practices](../best-practices/direct-lake.md)
- [Direct Lake overview](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview)
- [How Direct Lake works](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-how-it-works)
- [Direct Lake guardrails](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview#fabric-capacity-requirements)
