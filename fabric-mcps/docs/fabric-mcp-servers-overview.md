# Fabric MCP Servers overview - official Microsoft Learn reference

Microsoft Learn: [Fabric MCP Servers overview](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/what-is-fabric-mcp-server)

Microsoft Learn directory: [Choose a Microsoft Fabric MCP server](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/fabric-mcp-servers-list)

Local Fabric MCP Server source: [microsoft/mcp - Fabric.Mcp.Server](https://github.com/microsoft/mcp/tree/main/servers/Fabric.Mcp.Server)

## What it is

Official Microsoft Fabric REST API documentation that explains how AI agents can interact with Fabric through Model Context Protocol (MCP) servers.

Use it with the server directory as the canonical reference for choosing between Fabric Core, Fabric IQ, workload-specific remote servers, and local development servers.

Related local cards:

- [Fabric Core MCP Server](fabric-core-mcp-server.md)
- [Fabric IQ MCP Server](fabric-iq-mcp-server.md)
- [Fabric Data Warehouse MCP Server](fabric-data-warehouse-mcp-server.md)
- [Eventhouse MCP Server](eventhouse-mcp-server.md)
- [Activator MCP Server](activator-mcp-server.md)
- [Operations Agent MCP Server](operations-agent-mcp-server.md)
- [Ontology MCP Server](ontology-mcp-server.md)
- [Power BI Consumption MCP Server](power-bi-consumption-mcp-server.md)
- [Power BI Authoring MCP Server](power-bi-authoring-mcp-server.md)
- [Fabric MCP Server (local)](fabric-mcp-server-local.md)
- [DataFactory MCP Server](datafactory-mcp-server-local.md)
- [Real-Time Intelligence MCP Server local](real-time-intelligence-mcp-server-local.md)

## What it covers

| Area | Why it matters |
|------|----------------|
| MCP positioning | Explains MCP as a standard interface that lets AI agents discover tools, authenticate, call Fabric APIs, and return structured responses. |
| Fabric Core MCP Server | Describes the remote server for workspace, item, permission, and folder operations through Fabric REST APIs with Entra ID authentication and audit logging. |
| Fabric IQ MCP Server | Describes read-only Power BI report and semantic model discovery, metadata inspection, value search, and DAX execution. |
| Local Fabric MCP Server | Describes the local open-source development server for API documentation, OneLake operations, Fabric item creation, local file access, and offline docs/best-practice tooling. |
| Server directory | Points to workload-specific MCP servers for Data Warehouse, Eventhouse, Activator, Operations agents, Ontology, Data Factory, Real-Time Intelligence, and Power BI. |
| Server comparison | Helps decide when to use remote Core, local Fabric MCP Server, workload-specific remote servers, or local development servers together. |
| Security model | Reinforces that MCP servers require authentication, enforce Fabric RBAC, don't elevate privileges, and rely on token-based access. |

## How to use it with this guide

Use this document and the server directory as the starting point for Fabric MCP architecture decisions. Link to the Microsoft Learn articles when explaining supported server options, security expectations, and the difference between remote administration-style operations, workload-specific data tools, and local development workflows.

Validate release status, tenant settings, MCP client support, authentication flow, audit requirements, local file-system exposure, triggered actions, and customer data handling before using any server in a customer delivery plan.
