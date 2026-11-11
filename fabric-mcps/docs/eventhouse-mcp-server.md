# Eventhouse MCP Server

**Source:** [Get started with the remote MCP server for Eventhouses](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-remote-eventhouse)

Item-scoped endpoint: `https://api.fabric.microsoft.com/v1/mcp/dataPlane/workspaces/<workspace-id>/items/<item-id>/kqlEndpoint`

Global endpoint: `https://api.fabric.microsoft.com/v1/mcp/dataPlane/kqlEndpoint`

## What it is

The Eventhouse MCP server is a remote preview MCP endpoint for Microsoft Fabric Real-Time Intelligence eventhouses and KQL databases.

Use this when an AI agent needs to discover KQL database schemas, generate KQL, execute KQL queries, sample data, or reason over real-time and historical data in an Eventhouse without installing a local server.

Local counterpart: use the [Real-Time Intelligence MCP Server local](real-time-intelligence-mcp-server-local.md) when the workflow needs a self-hosted local server or broader RTI access across Eventhouse, Eventstream, Activator, maps, and Azure Data Explorer.

## Best fit

| Use case | Why this server fits |
|----------|----------------------|
| Eventhouse exploration | Discovers KQL database schemas and metadata dynamically |
| Natural-language to KQL workflows | Uses the NL2KQL framework to translate questions into KQL |
| Real-time investigation | Queries real-time and historical Eventhouse data from an MCP client |
| Azure Data Explorer compatibility | Supports optional `clusterUrl` and `databaseName` parameters for ADX-backed scenarios |

## Guardrails

- Confirm the user has read or query permissions on the KQL database.
- Prefer the item-scoped endpoint when the workflow should stay anchored to one approved Eventhouse.
- When using the global endpoint, require explicit `workspaceId` and `itemId` in tool calls.
- Enable Copilot in Fabric when schema-fetching support is required; otherwise the server might only run KQL queries.
- Review generated KQL before running expensive or broad queries.
- Treat samples and query results as customer data; review MCP client and model-provider data handling.

## References

- [Get started with the remote MCP server for Eventhouses](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-remote-eventhouse)
- [What is MCP in Real-Time Intelligence?](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-overview)
- [Real-Time Intelligence MCP Server local](real-time-intelligence-mcp-server-local.md)
