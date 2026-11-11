# Power BI connectivity decisions

[Solutions catalog](../README.md)

| Detail | Value |
|--------|-------|
| Type | Guide |
| Primary workload | Power BI |
| Related workloads | OneLake, Data Warehouse, Data Engineering |
| Public starting point | [FabricCAT: Modern Power BI Architecture Choices](https://github.com/microsoft/FabricCAT/blob/main/Direct%20Lake_DirectQuery/Modern%20Power%20BI%20Architecture%20Choices.pdf) |

## Scenario

A team is modernizing a Power BI estate or choosing connectivity for a new semantic model. The decision must account for source location, security, freshness, performance, and operating cost.

## Recommended approach

Start with a governed Gold star schema in OneLake and evaluate Direct Lake on OneLake for a new Fabric-native semantic model. Use the FabricCAT paper as a decision aid, then confirm current capabilities in Microsoft Learn.

1. Record business freshness and latency targets, source-access restrictions, and who must enforce row and column security.
2. Confirm that the source resolves to supported Delta tables, directly or through supported shortcuts. Avoid adding data copies solely for BI connectivity.
3. Select Direct Lake, Import, DirectQuery, or a supported composite design against those constraints. Document why an alternative to the preferred Direct Lake design is necessary.
4. Test realistic queries, representative identities, refresh or framing behavior, and capacity headroom before committing.

## Prerequisites and watch-outs

- Confirm capacity eligibility, author and viewer licensing, and permissions for the chosen mode.
- Direct Lake on OneLake does not support DirectQuery fallback. Direct Lake on SQL can fall back through its SQL analytics endpoint; test the applicable security and fallback behavior.
- SQL endpoint security and OneLake security are not interchangeable. Select the connection identity and access mode deliberately.
- Import can be a valid choice when its refresh window and data volume fit. DirectQuery needs a measured source-performance and security rationale, not just a preference for avoiding refresh.
- Keep the PDF upstream. Product behavior can change independently of the paper's publication.

## Validation before use

- Verify that test users see only their authorized data through every permitted access path.
- Compare cold and warm query performance, concurrency, and freshness against agreed targets.
- Record the selected mode and operational owner, including fallback monitoring where applicable.

## Related guidance

- [Direct Lake best practices](../../best-practices/direct-lake.md)
- [Semantic modeling](../../best-practices/semantic-modeling.md)
- [DirectQuery to external systems: trade-offs](../../anti-patterns/directquery-to-external.md)

## Microsoft Learn

- [Direct Lake overview](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview)
- [Develop Direct Lake semantic models](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-develop)
- [How Direct Lake works](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-how-it-works)
