# IQ supply-chain demo

[Solutions catalog](../README.md)

| Detail | Value |
|--------|-------|
| Type | Demo |
| Primary workload | Fabric IQ |
| Related workloads | OneLake, Microsoft Foundry, Microsoft 365 Copilot, Copilot Studio |
| Public starting point | [Microsoft IQ Solution Accelerator](https://github.com/microsoft/microsoft-iq-solution-accelerator) |

> **Evaluation scope:** The upstream accelerator uses synthetic data and includes preview dependencies. It is intended for evaluation, experimentation, and demonstrations, not as evidence of production readiness.

## Scenario

A supplier disruption requires a team to combine inventory and demand data, business relationships, contract knowledge, and collaboration context before choosing a response.

## Recommended approach

Use the public accelerator to demonstrate a governed decision-support loop, keeping data, business context, and action approval separate.

1. Review the upstream architecture, setup, permissions, and cost guidance before deploying to an isolated environment.
2. Use the supplied synthetic scenario. Keep analytical data in OneLake and establish the business entities and relationships used by Fabric IQ.
3. Follow the accelerator's Foundry and Copilot configuration to combine business-data answers with document context and coordinated workflows.
4. Trace a disruption from evidence to business impact and a proposed response. Require human approval for consequential actions.
5. Evaluate customization gaps before replacing sample sources or connecting operational systems.

## Prerequisites and watch-outs

- Confirm Azure deployment and role-assignment permissions, Fabric capacity and tenant settings, and the licenses and access required by the selected Foundry, Microsoft 365, and Copilot Studio components.
- **Ontology is preview.** The accelerator also identifies preview integration dependencies; verify each one instead of treating the complete solution as GA.
- Verify the identity and data-handling boundary of every connected service. A shared business vocabulary does not grant source permissions.
- Do not add an ontology to a single-domain BI question that an existing semantic model already answers adequately.
- Review runtime and evaluation costs and plan resource cleanup before deployment. Do not copy internal documents or real supplier information into a public demo.

## Validation before use

- Reconcile a sample answer to its underlying records and document evidence.
- Test unauthorized-access and ambiguous-question cases as well as the happy path.
- Confirm that recommendations remain distinguishable from approved actions, and remove only the demo resources created for the evaluation.

## Related guidance

- [Ontologies and Fabric IQ](../../fabric-data-agents/ontologies-fabric-iq.md)
- [Data architecture for agents](../../fabric-data-agents/data-architecture-for-agents.md)
- [Data Agent consumption patterns](../../fabric-data-agents/consumption-patterns.md)

## Microsoft Learn

- [What is ontology (preview)?](https://learn.microsoft.com/en-us/fabric/iq/ontology/overview)
- [Add a Fabric data agent as a tool in Copilot Studio](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-microsoft-copilot-studio-tool)
