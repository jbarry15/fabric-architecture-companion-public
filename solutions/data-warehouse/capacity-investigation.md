# Warehouse capacity investigation

[Solutions catalog](../README.md)

| Detail | Value |
|--------|-------|
| Type | Guide |
| Primary workload | Data Warehouse |
| Related workloads | Power BI, capacity administration |
| Public starting point | [Microsoft Learn: correlate capacity metrics with Warehouse Query Insights](https://learn.microsoft.com/en-us/fabric/data-warehouse/correlate-capacity-metrics-query-insights) |

## Scenario

A capacity spike or throttling event coincides with Warehouse activity. The team needs evidence to decide whether to tune queries, adjust scheduling, or resize capacity.

## Recommended approach

Use the report linked from the Microsoft Learn walkthrough to align Capacity Metrics and Warehouse Query Insights on the same time axis before changing capacity.

1. Identify the affected capacity, warehouse, and time window in Capacity Metrics.
2. Follow the walkthrough to configure the report against the correct Capacity Metrics semantic model and approved warehouses.
3. Inspect overlapping queries, duration, allocated CPU, scan volume, status, and SQL text. Use a request's distributed statement ID to investigate it in Query Insights.
4. Change one tuning or scheduling factor, then compare equivalent workload windows.

## Public repository reference

The [Fabric DW Query Capacity Correlation repository](https://github.com/mariyaali/fabric-dw-query-capacity-correlation) provides another public report implementation for this scenario. Keep its assets upstream and review reuse terms before copying them.

Use Microsoft Learn as the authority where the README differs: correlate by warehouse and overlapping time window, not by joining the two services' identifiers, and verify the actual overlapping telemetry history rather than assuming a shared 30-day window.

## Prerequisites and watch-outs

- The walkthrough requires Power BI Desktop with PBIP support, PowerShell, Azure CLI, access to the Capacity Metrics semantic model, and Read and Query Insights access to each warehouse.
- Align time zones and use the overlapping telemetry period; the two sources have different retention and reporting latency.
- **Do not join Capacity Metrics Operation ID to Query Insights distributed statement ID.** They are different identifiers. Correlate the resolved warehouse and overlapping time window.
- Query CPU and capacity consumption are different measurements. Time overlap alone does not establish causality or exact per-query CU consumption.
- The imported report model caches SQL text and user identities. Review source scope, report access, and service refresh credentials before sharing it.

## Validation before use

- Confirm that a known request appears in the expected warehouse and time window.
- Verify that missing warehouses reflect the discovery account's permissions, not an assumption of tenant-wide coverage.
- Record before/after evidence without exposing SQL literals, identities, or operational metadata in public artifacts.

## Related guidance

- [Data Warehouse best practices](../../best-practices/data-warehouse.md)
- [Capacity planning best practices](../../fabric-quota-capacity-planning/best-practices.md)

## Microsoft Learn

- [Correlation walkthrough and report download](https://learn.microsoft.com/en-us/fabric/data-warehouse/correlate-capacity-metrics-query-insights)
- [Fabric Capacity Metrics app](https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app)
