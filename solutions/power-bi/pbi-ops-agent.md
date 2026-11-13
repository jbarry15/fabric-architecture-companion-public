# Power BI Ops Agent

[Solutions catalog](../README.md)

| Detail | Value |
|--------|-------|
| Type | Demo |
| Primary workload | Power BI |
| Related workloads | Real-Time Intelligence, Microsoft Foundry, Microsoft Teams |
| Public starting point | [torentlikher-msft/pbi-ops-agent](https://github.com/torentlikher-msft/pbi-ops-agent) |

> **Proof of concept:** The upstream project explicitly requires thorough testing. This is not a production support commitment.

## Scenario

A model owner wants slow-query telemetry and AI-assisted diagnostic recommendations delivered through a Teams personal chat.

## Recommended approach

Evaluate the upstream workflow with one test model and approved recipients. Its design combines Fabric monitoring data, Azure Functions, Cosmos DB, Foundry, and a Teams bot.

The upstream agent recommends changes rather than modifying models, refreshing datasets, or deploying fixes. Keep any remediation in a separate reviewed process.

## Prerequisites and watch-outs

- Prepare existing semantic-model telemetry, approved Azure resources and model availability, Fabric access, Entra consent, and Teams app administration. The installer does not create the source telemetry for you.
- Read-only model diagnostics still involve query execution, conversation state, and data movement. Review telemetry content, token handling, recipient mapping, and retention across services.
- Do not infer this application's API access from workspace role names alone. Validate every selected API, identity, and tenant setting against current documentation.
- Review costs, deployment scripts, and reuse terms before installing. AI recommendations are hypotheses to test, not automatic root-cause findings.

## Validation before use

- Verify an observed slow query against the original telemetry.
- Test recipient isolation, sign-in, and denial of unauthorized access.
- Confirm that recommendations require review and that no model-changing action occurs.

## Related guidance

- [Semantic modeling](../../best-practices/semantic-modeling.md)
- [Real-Time Intelligence](../../best-practices/real-time-intelligence.md)

## Microsoft Learn

- [Semantic model operation logs](https://learn.microsoft.com/en-us/fabric/enterprise/powerbi/semantic-model-operations)
- [Workspace monitoring overview and data-handling considerations](https://learn.microsoft.com/en-us/fabric/fundamentals/workspace-monitoring-overview)
