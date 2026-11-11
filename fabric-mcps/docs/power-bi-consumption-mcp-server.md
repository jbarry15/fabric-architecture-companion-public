# Power BI Consumption MCP Server

**Source:** [Power BI Consumption MCP server setup](https://learn.microsoft.com/en-us/power-bi/developer/mcp/remote-mcp-server-get-started)

Endpoint: `https://api.fabric.microsoft.com/v1/mcp/powerbi`

## What it is

The Power BI Consumption MCP server is a remote preview endpoint for asking questions over Power BI semantic models from MCP-compatible clients.

It can generate and execute DAX while respecting the signed-in user's permissions and security policies.

> For new semantic model and report consumption scenarios, Microsoft recommends the [Fabric IQ MCP Server](fabric-iq-mcp-server.md). Use this card mainly when you need the earlier Power BI consumption endpoint or are reviewing existing setups.

## Best fit

| Use case | Why this server fits |
|----------|----------------------|
| Existing Power BI MCP consumption setups | Keeps earlier endpoint guidance visible for migration and review |
| Semantic model Q&A | Can execute DAX against a semantic model the user can access |
| Report-aware context | Can retrieve report metadata to guide query generation |
| Copilot-assisted semantic model exploration | Exposes tools for schema, report metadata, query generation, and execution |

## Available tools

| Tool | What it does |
|------|--------------|
| Execute Query | Runs a DAX query against a semantic model |
| Get Semantic Model Schema | Retrieves tables, columns, measures, relationships, and AI-optimized metadata when configured |
| Get Report Metadata | Retrieves report structure, pages, visuals, filters, and bindings |
| Generate Query | Uses Copilot in Power BI to create optimized DAX from natural language |

## Guardrails

- Prefer Fabric IQ MCP for new report and semantic model consumption workflows.
- Confirm the Power BI tenant setting for the Power BI MCP endpoint is enabled.
- Require Build permission on the target semantic model.
- Review RLS behavior and service-principal constraints before using this in production.
- Disable `Generate Query` if you do not want to consume Copilot capacity.
- Keep semantic-model IDs, sample results, and chat transcripts within approved data-handling boundaries.

## References

- [Power BI Consumption MCP server setup](https://learn.microsoft.com/en-us/power-bi/developer/mcp/remote-mcp-server-get-started)
- [Fabric IQ MCP Server](fabric-iq-mcp-server.md)
