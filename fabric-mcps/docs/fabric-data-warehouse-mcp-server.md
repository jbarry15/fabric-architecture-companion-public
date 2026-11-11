# Fabric Data Warehouse MCP Server - remote SQL endpoint MCP

Microsoft Learn: [Connect to Fabric Data Warehouse MCP Server](https://learn.microsoft.com/en-us/fabric/data-warehouse/data-warehouse-mcp-server)

Global endpoint: `https://api.fabric.microsoft.com/v1/mcp/dataPlane/sqlEndpoint`

Item-scoped endpoint: `https://api.fabric.microsoft.com/v1/mcp/dataPlane/workspaces/<workspace-id>/items/<item-id>/sqlEndpoint`

## What it is

Fabric Data Warehouse MCP Server is a hosted Microsoft Fabric MCP endpoint that lets AI agents run approved T-SQL against a Fabric Warehouse or SQL analytics endpoint through natural language.

Use it when an architecture or delivery scenario needs agent-assisted warehouse exploration, T-SQL generation, query explanation, metadata inspection, or operational query investigation against Fabric Data Warehouse context.

## What it covers

| Area | Why it matters |
|------|----------------|
| Remote MCP access | Connects MCP-compatible hosts such as Visual Studio Code to Fabric Warehouse or SQL analytics endpoint through a hosted HTTP endpoint. |
| Global and item-scoped endpoints | Supports flexible warehouse selection through prompts or a connection bound to a specific workspace and warehouse item. |
| `executeSQL` tool | Exposes one tool that runs approved T-SQL and returns results to the agent. |
| SQL metadata workflows | Lets agents discover schemas, tables, views, columns, sessions, and query activity by generating T-SQL against supported metadata views. |
| User identity and permissions | Uses the signed-in user's identity and respects Fabric workspace, item, and SQL permissions. |

## Best-fit scenarios

- Exploring tables, views, schemas, and column metadata in a Fabric Warehouse or SQL analytics endpoint.
- Generating, explaining, and running read-only T-SQL during design, troubleshooting, or enablement.
- Reviewing warehouse query activity or failures where supported system views are available.
- Anchoring a Copilot agent to one warehouse by using the item-scoped endpoint.
- Validating warehouse logic before promoting it into source-controlled scripts, notebooks, deployment pipelines, or operational runbooks.

## Watch-outs

- The server exposes `executeSQL`, not separate schema-discovery or warehouse-admin tools. Ask the agent to use T-SQL metadata queries when exploring objects.
- Review generated T-SQL before approving `executeSQL`, especially for `CREATE`, `ALTER`, `DROP`, `INSERT`, `UPDATE`, or `DELETE`.
- Start with read-only queries and least-privilege access for production warehouses.
- The server doesn't bypass Fabric security. Permission failures usually mean the user lacks the required workspace, item, or SQL permission.
- Prefer the item-scoped endpoint when a workflow should stay anchored to one approved warehouse.
