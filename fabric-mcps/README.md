# Fabric MCP Resources

Curated Model Context Protocol (MCP) servers and agent-facing tool resources for Microsoft Fabric architecture, authoring, modeling, validation, and operations.

Use this area for MCP servers that can connect AI agents to Fabric or Power BI capabilities. Keep external repositories as canonical unless this guide needs a reviewed point-in-time prompt, setup pattern, or customer delivery runbook.

## Guidance

| Document | Use it for |
|----------|------------|
| [Best practices](best-practices.md) | Selecting the right Fabric MCP server, controlling write access, protecting data, validating output, and operationalizing reusable MCP workflows |
| [Anti-patterns](anti-patterns.md) | Common MCP mistakes around overbroad tools, permissions, skipped confirmations, local file exposure, preview assumptions, and weak validation |
| [Official Microsoft Fabric MCP server directory](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/fabric-mcp-servers-list) | Current Microsoft Learn directory of Fabric, Power BI, Data Factory, Real-Time Intelligence, and related MCP server options |

## Resource catalog

| Resource | Hosting | Primary workload | What it enables | Local card |
|----------|---------|------------------|-----------------|------------|
| [Fabric MCP Servers overview](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/what-is-fabric-mcp-server) | Remote + local overview | Fabric MCPs | Official Microsoft Learn overview of Fabric's remote Core MCP Server and local Fabric MCP Server, including positioning, capabilities, security, and server-selection guidance | [fabric-mcp-servers-overview.md](docs/fabric-mcp-servers-overview.md) |
| [Fabric Core MCP Server](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/core-remote/get-started-core) | Remote | Fabric REST API operations | Remote MCP server for workspace, item, item relations, folder, and permission operations through Fabric REST APIs with Entra ID OAuth, Fabric RBAC, and audit logging | [fabric-core-mcp-server.md](docs/fabric-core-mcp-server.md) |
| [Fabric IQ MCP Server](https://learn.microsoft.com/en-us/fabric/iq/connectors/fabric-iq-mcp) | Remote | Power BI reports and semantic models | GA MCP server for read-only discovery, metadata inspection, value search, and DAX query execution against Power BI content the signed-in user can access | [fabric-iq-mcp-server.md](docs/fabric-iq-mcp-server.md) |
| [Fabric Data Agent MCP Server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server) | Remote | Data agents and scoped business Q&A | Published data agent endpoint for MCP clients to ask governed business questions against scoped Fabric data sources, including ontology-grounded sources | [fabric-data-agent-mcp-server.md](docs/fabric-data-agent-mcp-server.md) |
| [Fabric Data Warehouse MCP Server](https://learn.microsoft.com/en-us/fabric/data-warehouse/data-warehouse-mcp-server) | Remote | Data Warehouse and SQL analytics endpoint | MCP server for approved T-SQL execution through the `executeSQL` tool against Fabric Warehouse or SQL analytics endpoint context | [fabric-data-warehouse-mcp-server.md](docs/fabric-data-warehouse-mcp-server.md) |
| [Eventhouse MCP Server](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-remote-eventhouse) | Remote | Real-Time Intelligence Eventhouse and KQL | Preview MCP server for Eventhouse schema discovery, natural-language-to-KQL, query execution, and data sampling | [eventhouse-mcp-server.md](docs/eventhouse-mcp-server.md) |
| [Activator MCP Server](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-remote-activator) | Remote | Real-Time Intelligence Activator | Preview MCP server for creating and managing monitoring rules, alerts, and Teams/email actions | [activator-mcp-server.md](docs/activator-mcp-server.md) |
| [Operations Agent MCP Server](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-overview) | Remote | Real-Time Intelligence operations agents | MCP server for operations-agent instruction configuration, playbook generation, and monitoring management | [operations-agent-mcp-server.md](docs/operations-agent-mcp-server.md) |
| [Ontology MCP Server](https://learn.microsoft.com/en-us/fabric/iq/ontology/how-to-use-ontology-mcp-server) | Remote | Fabric IQ ontology | Preview MCP endpoint for exposing ontology entities, relationships, and definitions to AI agents | [ontology-mcp-server.md](docs/ontology-mcp-server.md) |
| [Power BI Consumption MCP Server](https://learn.microsoft.com/en-us/power-bi/developer/mcp/remote-mcp-server-get-started) | Remote | Power BI semantic model consumption | Preview MCP server for semantic model Q&A and DAX execution; prefer Fabric IQ MCP for new consumption workflows | [power-bi-consumption-mcp-server.md](docs/power-bi-consumption-mcp-server.md) |
| [Power BI Authoring MCP Server](https://learn.microsoft.com/en-us/power-bi/developer/mcp/power-bi-authoring-mcp) | Remote + local | Power BI semantic model authoring | Hosted preview and local GA authoring server for creating, editing, validating, and refactoring semantic models | [power-bi-authoring-mcp-server.md](docs/power-bi-authoring-mcp-server.md) |
| [Fabric MCP Server (local)](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/pro-dev-local/get-started-local) | Local | Fabric development workflows | Open-source MCP server for API specs, best-practice guidance, OneLake file operations, item creation, local file integration, and offline development workflows | [fabric-mcp-server-local.md](docs/fabric-mcp-server-local.md) |
| [DataFactory MCP Server](https://learn.microsoft.com/en-us/fabric/data-factory/pipelines-manage-vs-code) | Local | Data Factory pipelines and Airflow jobs | .NET tool for creating, updating, running, scheduling, and monitoring Fabric pipelines and Apache Airflow jobs from VS Code | [datafactory-mcp-server-local.md](docs/datafactory-mcp-server-local.md) |
| [Real-Time Intelligence MCP Server local](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-local-server) | Local | Real-Time Intelligence development workflows | Open-source MCP server for Eventhouse, Eventstream, Activator, map, and Azure Data Explorer workflows | [real-time-intelligence-mcp-server-local.md](docs/real-time-intelligence-mcp-server-local.md) |

## Remote vs local coverage

Some Fabric workloads have both Microsoft-hosted remote MCP servers and local/self-hosted MCP servers. Treat them as separate options with different operating models.

| Workload area | Remote / hosted option | Local / self-hosted option | Notes |
|---------------|------------------------|----------------------------|-------|
| Fabric workspace, item, item relations, folder, and permission operations | [Fabric Core MCP Server](docs/fabric-core-mcp-server.md) | [Fabric MCP Server local](docs/fabric-mcp-server-local.md) | Remote Core is for supported REST API operations, including upstream/downstream item relations where available; local Fabric adds API docs, OneLake, item creation, local files, and development tooling |
| Eventhouse and KQL | [Eventhouse MCP Server](docs/eventhouse-mcp-server.md) | [Real-Time Intelligence MCP Server local](docs/real-time-intelligence-mcp-server-local.md) | Remote Eventhouse is KQL/Eventhouse scoped; local RTI covers Eventhouse plus broader RTI/ADX workflows |
| Activator | [Activator MCP Server](docs/activator-mcp-server.md) | [Real-Time Intelligence MCP Server local](docs/real-time-intelligence-mcp-server-local.md) | Remote Activator is artifact-scoped; local RTI covers Activator as part of broader local RTI tooling |
| Operations agents | [Operations Agent MCP Server](docs/operations-agent-mcp-server.md) | No distinct local server listed in the official directory | Use the RTI overview to validate current support before delivery |
| Power BI semantic model consumption | [Fabric IQ MCP Server](docs/fabric-iq-mcp-server.md) or [Power BI Consumption MCP Server](docs/power-bi-consumption-mcp-server.md) | No direct local consumption counterpart | Prefer Fabric IQ MCP for new report and semantic model consumption workflows |
| Power BI semantic model authoring | [Power BI Authoring MCP Server](docs/power-bi-authoring-mcp-server.md) | [Local deployment](repos/powerbi-modeling-mcp.md) | Hosted authoring targets Fabric workspace models; local authoring also supports Power BI Desktop, PBIP, and TMDL files |
| Data Factory pipelines and Airflow jobs | No remote server listed in the official directory | [DataFactory MCP Server](docs/datafactory-mcp-server-local.md) | Local .NET tool used from VS Code/Copilot |
| Data Warehouse and SQL analytics endpoint | [Fabric Data Warehouse MCP Server](docs/fabric-data-warehouse-mcp-server.md) | No local Fabric Data Warehouse MCP counterpart listed | Use item-scoped or global remote SQL endpoint |
| Data agents | [Fabric Data Agent MCP Server](docs/fabric-data-agent-mcp-server.md) | No local counterpart listed | Published data agents expose the remote MCP endpoint |
| Ontology | [Ontology MCP Server](docs/ontology-mcp-server.md) | No local counterpart listed | Item-scoped preview endpoint for ontology context |

**Fabric IQ client previews:** The [Ignite 2026 announcements](https://news.microsoft.com/events/ignite/2026/book-of-news/) describe ontology context in GitHub Copilot and separate connectors for querying Power BI semantic models from ChatGPT and Claude. These are different integration surfaces. Before piloting, verify tenant availability, the identity used to query data, and each client's data-handling terms.

## Related integrations and samples

| Resource | Hosting | Use it for |
|----------|---------|------------|
| [Fabric API for GraphQL sample MCP server](https://learn.microsoft.com/en-us/fabric/data-engineering/api-graphql-local-model-context-protocol) | Local sample | Exposing Fabric API for GraphQL schema discovery and queries to agents |
| [SQL MCP Server / Data API builder](https://learn.microsoft.com/en-us/azure/data-api-builder/mcp/) | Customer-hosted | MCP integration for SQL databases, including SQL database in Fabric scenarios |
| [SQL database in Fabric AI apps](https://learn.microsoft.com/en-us/fabric/database/sql/use-case-ai-application) | Customer-hosted pattern | SQL database in Fabric AI application patterns and SQL MCP placement |
| [OneLake MCP tools](https://learn.microsoft.com/en-us/fabric/onelake/onelake-local-mcp) | Local, via Fabric MCP Server | OneLake tools included in the local Fabric MCP Server rather than a separate server |

## Placement guidance

- Use `fabric-mcps/` for MCP servers and agent-facing tool servers that expose Fabric or Power BI capabilities to AI agents.
- Use `fabric-skills/` for reusable Copilot skills, prompts, and agent instructions.
- Use `toolbox/` for broader Fabric tools, scripts, accelerators, and operational utilities.
- Keep Microsoft Learn links as the canonical docs source; local cards should summarize placement and reuse guidance, not copy full articles.

## Review checklist

Before using an MCP server in a customer delivery plan, validate:

1. Current release status and supportability.
2. MCP client compatibility, transport, and required runtime.
3. Authentication mode, tenant settings, and least-privilege permissions.
4. Whether the server can perform write or destructive operations.
5. Human approval, backup, and rollback expectations.
6. Customer data, metadata, and prompt-sharing handling with the configured LLM provider.
