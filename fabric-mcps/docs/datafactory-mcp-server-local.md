# DataFactory MCP Server

**Sources:** [Manage pipelines in Fabric from VS Code](https://learn.microsoft.com/en-us/fabric/data-factory/pipelines-manage-vs-code) and [Manage Apache Airflow jobs in Fabric from VS Code](https://learn.microsoft.com/en-us/fabric/data-factory/apache-airflow-jobs-manage-vs-code)

Install: `dotnet tool install --global Microsoft.DataFactory.MCP`

Command: `datafactorymcp`

Pipeline tools flag: `--pipeline`

## What it is

The DataFactory MCP Server is a local MCP server distributed as the `Microsoft.DataFactory.MCP` .NET tool. It exposes Fabric Data Factory operations to MCP clients such as GitHub Copilot in VS Code.

Use this when an agent needs to create, configure, run, monitor, or manage Fabric pipelines and Apache Airflow jobs from a local development environment.

## Best fit

| Use case | Why this server fits |
|----------|----------------------|
| Pipeline creation and updates | Can create pipeline items and update pipeline definitions from natural-language prompts |
| Pipeline execution and monitoring | Can run pipelines, retrieve run status, and inspect failures |
| Pipeline scheduling | Can create and list recurring schedules for Fabric pipelines |
| Apache Airflow job lifecycle | Can create, update, monitor, list, and delete Airflow jobs |
| DAG definition management | Can read and update Airflow DAG and scheduler configuration |

## Guardrails

- Requires .NET 10.0 or later and a Fabric workspace with assigned capacity.
- Use the `--pipeline` flag when pipeline management tools are required.
- Authenticate interactively with an Entra ID account that has permission to create and run workspace items.
- Review generated pipeline JSON, schedules, runtime parameters, and delete operations before approval.
- Prefer recycle-bin deletion for Airflow jobs and other items unless permanent deletion is explicitly required.
- Keep connection names, workspace IDs, and operational details out of shared prompts when they identify customer environments.

## References

- [Manage pipelines in Fabric from VS Code](https://learn.microsoft.com/en-us/fabric/data-factory/pipelines-manage-vs-code)
- [Manage Apache Airflow jobs in Fabric from VS Code](https://learn.microsoft.com/en-us/fabric/data-factory/apache-airflow-jobs-manage-vs-code)
