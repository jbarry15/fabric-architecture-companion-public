# Fabric IQ and Copilot finance demo

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Demo |
| Primary category | Fabric AI, IQ, and Ontology |
| Related workloads | Power BI, Microsoft 365 Copilot, GitHub Copilot |
| Public starting point | [liamhowlett/fabric-iq-copilot-demo](https://github.com/liamhowlett/fabric-iq-copilot-demo) |

> **Setup boundary:** The upstream README references an employee-only setup guide. This card does not reproduce or link to that guide, and the public materials should not be treated as a complete tenant-provisioning walkthrough.

## Scenario

A presenter wants to demonstrate how a governed finance semantic model and synthetic workplace context can support related questions across Microsoft 365 Copilot, Copilot Cowork, and GitHub Copilot.

## Recommended approach

Use the public model, sample-data descriptions, and demo script as a reference. Let the semantic model define the numeric results and use synthetic documents and conversations for explanatory context.

Start with a small, approved demo scope. Reconcile answers across available surfaces, then review any proposed measure change against the model before applying it to a disposable copy.

## Prerequisites and watch-outs

- Independently verify licensing, tenant provisioning, permissions, regional availability, and tool setup for each selected surface. Do not infer capacity or cost requirements from the demo's marketing claims.
- The remote Fabric IQ MCP toolset is read-only; model-authoring steps use a separate tool path and require explicit approval.
- Retrieval depends on accessible, indexed content. Do not promise identical answers, timing, or visibility across clients.
- Use synthetic material only. Review reuse terms before copying assets, and do not stage real employee conversations or confidential finance data for a public demo.

## Validation before use

- Reconcile a sample answer to the model under the intended date and business filters.
- Confirm which public setup steps work in the target tenant and disclose any gaps.
- Test data visibility and review generated calculations before saving model changes.

## Related guidance

- [Semantic modeling](../../../best-practices/semantic-modeling.md)
- [Fabric IQ MCP reference](../../../fabric-mcps/docs/fabric-iq-mcp-server.md)

## Microsoft Learn

- [Fabric IQ MCP capabilities, permissions, and setup](https://learn.microsoft.com/en-us/fabric/iq/connectors/fabric-iq-mcp)
