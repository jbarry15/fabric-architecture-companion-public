# Rayfin Onboarding 2026

[Fabric Apps solutions](../README.md)

| Detail | Value |
|--------|-------|
| Type | Workshop |
| Primary workload | Fabric Apps |
| Related workloads | Databases in Fabric, Power BI, Fabric data agents |
| Public starting point | [mehrsa/Rayfin-Onboarding2026](https://github.com/mehrsa/Rayfin-Onboarding2026) |

## Scenario

A developer wants a guided exercise that builds a work-order management app, evolves it with GitHub Copilot CLI, and explores application data through Fabric analytics.

## Recommended approach

Use the public lab as a learning path, with Microsoft Learn as the authority for current setup and product requirements.

1. Review the [lab instructions](https://github.com/mehrsa/Rayfin-Onboarding2026/blob/main/docs/README.md) and arrange Node.js, npm, Git, Azure CLI, Copilot CLI access, and an approved development workspace.
2. Bootstrap the template, inspect the generated application, and verify its Fabric deployment using sample data.
3. Review Copilot-generated feature and schema changes before applying them.
4. Follow the analytics exercises only after confirming the separate semantic-model and data-agent prerequisites.

## Prerequisites and watch-outs

- **Fabric Apps is preview.** A completed workshop is not production certification, regardless of the lab's deployment terminology.
- Some upstream setup directions are event-specific. Use the standard public Fabric setup documentation for your tenant rather than assuming the lab's environment or workspace settings apply.
- Do not assume a trial-workspace setup meets the data-agent extension's requirements. Microsoft Learn specifies paid F2 or higher, or eligible Power BI Premium P1 or higher with Fabric enabled, plus the required AI tenant settings and source read access.
- Local frontend development can use a Fabric-hosted backend and incur cloud costs. Review deployment targets, capacity, Copilot access, and cleanup before starting.
- Keep the implementation and lab assets upstream. Use sample or approved data, not confidential work orders or unreviewed agent-generated changes.

## Validation before use

- Verify the work-order workflow and access rules before extending the app.
- Confirm generated schema changes and the intended deployment target.
- Reconcile a sample analytic or agent answer to its underlying records; do not treat successful deployment as proof of answer quality.

## Related guidance

- [Microsoft Rayfin reference](microsoft-rayfin.md)
- [Fabric Apps best practices](../../../best-practices/fabric-apps.md)
- [Data-agent readiness and evaluation](../../fabric-ai-iq-ontology/data-agent-readiness-evaluation.md)

## Microsoft Learn

- [Fabric Apps overview and preview prerequisites](https://learn.microsoft.com/en-us/fabric/apps/overview)
- [Create and deploy a Fabric app with the CLI](https://learn.microsoft.com/en-us/fabric/apps/create-app-with-cli)
- [Fabric data agent creation and prerequisites](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent)
