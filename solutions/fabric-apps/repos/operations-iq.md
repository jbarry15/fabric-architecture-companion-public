# Operations IQ

[Fabric Apps solutions](../README.md)

| Detail | Value |
|--------|-------|
| Type | Reference implementation |
| Primary workload | Fabric Apps |
| Related workloads | Real-Time Intelligence, Data Engineering, Databases in Fabric, Microsoft Foundry |
| Public starting point | [slavatrofimov/OperationsIQ](https://github.com/slavatrofimov/OperationsIQ) |

## Scenario

An operations team wants to investigate time-series behavior, preserve visual evidence, and turn validated findings into monitored conditions.

## Recommended approach

Evaluate the public application with sample telemetry in an isolated environment. Its documented architecture combines a Fabric app, Eventhouse analytics, SQL application records, Spark discovery, a Foundry advisor, and Activator rules.

Use the upstream application and administrator guides for setup. Keep analytical findings separate from operational decisions, and require explicit approval before enabling actions or changing saved state.

## Prerequisites and watch-outs

- **Fabric Apps is generally available.** Confirm tenant enablement, capacity, and the support status of the other components selected for deployment.
- Deployment spans Fabric and Azure/Entra configuration. Review permissions, identity paths, connection settings, resource costs, and cleanup before running the orchestrator.
- AI explanations, statistical associations, and forecasts do not prove causation or safe operating limits. Validate conclusions with domain experts.
- Activator rules can notify or trigger actions. Test them without consequential downstream effects before enabling operational use.
- Keep code, assets, and playbooks upstream; review reuse terms and data-handling requirements before adoption.

## Validation before use

- Reconcile a sample analysis with the underlying time-series records.
- Test source access and application permissions with representative identities.
- Verify approval boundaries and rule behavior, including stop and recovery procedures.

## Related guidance

- [Fabric Apps best practices](../../../best-practices/fabric-apps.md)
- [Real-Time Intelligence](../../../best-practices/real-time-intelligence.md)

## Microsoft Learn

- [Fabric Apps overview and prerequisites](https://learn.microsoft.com/en-us/fabric/apps/overview)
