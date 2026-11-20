# Power BI Authoring MCP Server (local)

Source repo: [microsoft/powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp)

Demo video: [Power BI semantic modeling demo](https://aka.ms/power-modeling-mcp-demo)

## What it is

The [local Power BI Authoring MCP server](https://learn.microsoft.com/en-us/power-bi/developer/mcp/power-bi-authoring-mcp) is generally available. It connects AI agents to Power BI semantic models in Power BI Desktop, Fabric workspaces, or Power BI Project (PBIP/TMDL) files. The GitHub repository retains the `powerbi-modeling-mcp` name. The hosted option is a separate deployment of the same authoring server and is in preview.

Use it when an architecture or delivery scenario needs AI-assisted semantic modeling, DAX query validation, TMDL/PBIP workflows, model documentation, translations, bulk refactoring, or governed semantic model operations.

## What it covers

| Area | Why it matters |
|------|----------------|
| Semantic model authoring | Supports natural-language changes to tables, columns, measures, relationships, partitions, calculation groups, perspectives, translations, and security roles. |
| DAX validation | Lets agents execute and validate DAX queries against connected models. |
| TMDL/PBIP workflows | Enables agentic development over semantic model source files and deployable project structures. |
| Bulk model operations | Helps accelerate repetitive changes such as renaming, documentation, translations, refactoring, and parameterization. |
| Governance guardrails | Uses existing Power BI/Fabric permissions and supports read-only mode, write confirmations, and service-principal or interactive authentication. |

## How to use it with this guide

Use the [hosted-versus-local comparison](../docs/power-bi-authoring-mcp-server.md) to choose a deployment. The local server does not support macOS; on a Mac, use the hosted option for Fabric workspace models.

Verify MCP client support, XMLA requirements for workspace models, semantic model permissions, authentication mode, write-operation confirmations, model backups, and data handling before use.
