# Data-agent readiness and evaluation

[Solutions catalog](../README.md)

| Detail | Value |
|--------|-------|
| Type | Workshop |
| Primary workload | Fabric IQ - data agents |
| Related workloads | Power BI, Data Engineering, Data Science |
| Public starting point | [Microsoft Fabric Data Agent Workshop](https://github.com/microsoft/fabric-data-agent-workshop) |

## Scenario

A team can create a data agent, but needs to make its answers accurate, explainable, and repeatable before expanding access.

## Recommended approach

Start with one governed business domain and a curated semantic model. Use the public workshop to compare baseline and AI-ready models, inspect generated queries, and measure improvements before adding sources.

1. Follow the upstream setup instructions in an isolated workshop workspace.
2. Define a benchmark of questions, expected queries or answers, and acceptable latency.
3. Improve business metadata, AI schema, verified answers, and instructions. Inspect generated DAX and run steps to diagnose errors rather than tuning prompts blindly.
4. Compare configurations against the same benchmark. The workshop demonstrates a calibrated LLM judge and MLflow tracking; retain human review of representative answers.
5. Add the Lakehouse source only after the single-source baseline is understood, then repeat answer-quality and permission checks.

## Prerequisites and watch-outs

- The workshop lists paid Fabric F2 or higher, or eligible Power BI Premium capacity with Fabric enabled; Power BI Pro; workspace Contributor access or higher; and required tenant settings and item-creation permissions.
- **The data-agent evaluation SDK is preview.** Validate its current prerequisites and publish the agent to the stage being evaluated before running that evaluation path.
- Review cross-geography AI processing/storage settings and who can access questions, results, and evaluation traces.
- Evaluation scores are evidence, not production approval. A judge can be wrong, and a narrow benchmark can miss important failure cases.
- Keep notebooks, sample data, and installation instructions upstream. Verify installation availability and package versions before a workshop.

## Validation before use

- Compare baseline and tuned results using identical questions and recorded configurations.
- Include ambiguous questions, unsupported requests, and representative reader identities.
- Confirm source selection, metric definitions, authorized answers, and acceptable latency before publishing more broadly.

## Related guidance

- [Data Agent AI-ready checklist](../../fabric-data-agents/ai-ready-checklist.md)
- [Data Agent best practices](../../fabric-data-agents/data-agent-best-practices.md)
- [Consumption patterns](../../fabric-data-agents/consumption-patterns.md)

## Microsoft Learn

- [Evaluate your data agent (preview)](https://learn.microsoft.com/en-us/fabric/data-science/evaluate-data-agent)
- [Adopt an iterative process to improve your data agent](https://learn.microsoft.com/en-us/fabric/data-science/develop-iterative-process-data-agent)
