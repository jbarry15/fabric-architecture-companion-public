# Real-Time Intelligence MCP Server local

**Source:** [Get started with the local MCP server for Real-Time Intelligence](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-local-server)

Source repo: [microsoft/fabric-rti-mcp](https://github.com/microsoft/fabric-rti-mcp/)

## What it is

The local Real-Time Intelligence MCP Server is an open-source MCP server that you install, host, and manage. It lets MCP clients interact with Fabric Real-Time Intelligence and Azure Data Explorer resources.

Use this when a local agent workflow needs broader RTI development access than a single remote Eventhouse or Activator endpoint provides.

Remote counterparts include the [Eventhouse MCP Server](eventhouse-mcp-server.md), [Activator MCP Server](activator-mcp-server.md), and [Operations Agent MCP Server](operations-agent-mcp-server.md). Choose remote when a Microsoft-hosted, scoped endpoint fits the task; choose local when you need self-hosted development control or broader RTI/ADX coverage.

## Best fit

| Use case | Why this server fits |
|----------|----------------------|
| Local RTI development | Runs locally and exposes RTI tools through MCP |
| Eventhouse and ADX querying | Supports KQL query and schema-discovery workflows |
| Eventstream and Activator management | Can interact with eventstreams and Activator resources |
| Map workflows | Supports Fabric map resource operations |
| Offline or controlled hosting needs | You own installation, updates, and runtime environment |

## Guardrails

- Treat this as a local server with live Fabric reach; it can expose metadata and data to the MCP host and selected model provider.
- Install and run it only in a controlled project workspace.
- Use least-privilege access for RTI and ADX resources.
- Validate generated KQL and management operations before approval.
- Review the open-source repository instructions before workshops because install and tool details may change.

## References

- [Get started with the local MCP server for Real-Time Intelligence](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-local-server)
- [What is MCP in Real-Time Intelligence?](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-overview)
- [microsoft/fabric-rti-mcp](https://github.com/microsoft/fabric-rti-mcp/)
- [Eventhouse MCP Server](eventhouse-mcp-server.md)
- [Activator MCP Server](activator-mcp-server.md)
