# Fabric MCP Best Practices

Best practices for using Microsoft Fabric MCP servers and related MCP tooling in architecture, development, operations, and customer delivery workflows.

> **Canonical docs**: Start with [Choose a Microsoft Fabric MCP server](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/fabric-mcp-servers-list), then open the workload-specific setup article linked from that directory. Key entries include [Fabric MCP Servers overview](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/what-is-fabric-mcp-server), [Fabric Core MCP Server](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/core-remote/get-started-core), [Fabric IQ MCP server](https://learn.microsoft.com/en-us/fabric/iq/connectors/fabric-iq-mcp), [Data agent as MCP server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server), [Fabric Data Warehouse MCP Server](https://learn.microsoft.com/en-us/fabric/data-warehouse/data-warehouse-mcp-server), and [Fabric MCP Server local](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/pro-dev-local/get-started-local).

This guidance explicitly covers the Microsoft Learn Fabric MCP server options:

- **Fabric Core MCP Server** - remote Fabric REST API operations.
- **Fabric IQ MCP Server** - read-only Power BI report and semantic model discovery, metadata inspection, value search, and DAX query execution.
- **Fabric Data Agent MCP Server** - published data agent endpoint for governed natural-language Q&A over scoped Fabric data.
- **Fabric Data Warehouse MCP Server** - remote T-SQL execution against Fabric Warehouse and SQL analytics endpoint context.
- **Eventhouse MCP Server** - remote KQL schema discovery, NL2KQL, query execution, and data sampling.
- **Activator MCP Server** - remote monitoring-rule creation and management with Teams/email actions.
- **Operations Agent MCP Server** - remote operations-agent instructions, playbooks, and monitoring management.
- **Ontology MCP Server** - preview ontology context for entities, relationships, and definitions.
- **Power BI Consumption MCP Server** - preview semantic model Q&A endpoint; prefer Fabric IQ MCP for new consumption workflows.
- **Power BI Authoring MCP Server** - hosted or local semantic model authoring.
- **Fabric MCP Server (local)** - local development workflows, API docs/specs, OneLake operations, and extensibility.
- **DataFactory MCP Server** - local pipeline and Apache Airflow job management.
- **Real-Time Intelligence MCP Server (local)** - local RTI and Azure Data Explorer workflows.

## 1. Choose the MCP server by job-to-be-done

| Need | Preferred option | Why |
|------|------------------|-----|
| Workspace, item, permission, or folder management through Fabric REST APIs | Fabric Core MCP Server | Remote endpoint, OAuth 2.0, RBAC enforced, and Fabric audit logged |
| Lineage-aware item impact analysis using upstream/downstream dependencies | Fabric Core MCP Server | Uses Fabric REST API item relations where available, keeping impact checks tied to the connected identity's Fabric permissions |
| Power BI report and semantic model discovery, metadata inspection, value search, or DAX query execution | Fabric IQ MCP Server | Remote GA endpoint with read-only tools, delegated Entra authentication, existing Power BI permissions, and RLS/OLS enforcement |
| Governed business Q&A from an MCP client, Copilot Studio flow, or custom agent | Fabric Data Agent MCP Server | Published data agent exposes one scoped tool, using the data agent's source selection, instructions, and Fabric permissions |
| Warehouse or SQL analytics endpoint exploration, T-SQL generation, query explanation, or approved SQL execution | Fabric Data Warehouse MCP Server | Remote endpoint scoped to Fabric Warehouse context, with user permissions enforced and one explicit `executeSQL` approval surface |
| Eventhouse schema discovery, natural-language-to-KQL, data sampling, or KQL execution | Eventhouse MCP Server | Remote endpoint scoped to KQL/Eventhouse context and designed for Real-Time Intelligence query workflows |
| Activator monitoring rules, alerts, or Teams/email actions | Activator MCP Server | Remote endpoint scoped to a specific Activator artifact with rule-management tools |
| Operations-agent instructions, playbooks, or monitoring workflows | Operations Agent MCP Server | Remote Real-Time Intelligence endpoint for operations-agent configuration and monitoring management |
| Business entities, relationships, and ontology definitions | Ontology MCP Server | Item-scoped preview endpoint for exposing Fabric IQ ontology context to agents |
| Existing Power BI remote consumption endpoint workflows | Power BI Consumption MCP Server | Preview endpoint for semantic model Q&A; keep visible for existing setups while preferring Fabric IQ MCP for new work |
| Hosted semantic model authoring in Fabric workspaces | Power BI Authoring MCP Server | Hosted endpoint for model changes when local files/Desktop access are not required |
| Development workflows, API docs/specs, OneLake file operations, local file integration, or offline best-practice help | Fabric MCP Server (local) | Local subprocess with development tooling and open-source extensibility |
| Data Factory pipelines, schedules, run monitoring, or Apache Airflow job lifecycle | DataFactory MCP Server | Local .NET tool for Fabric Data Factory authoring and operations from VS Code |
| Local Real-Time Intelligence, Eventhouse, Eventstream, Activator, map, or ADX workflows | Real-Time Intelligence MCP Server (local) | Local open-source server for broader RTI development workflows |
| Power BI Desktop modeling, local TMDL/PBIP workflows, or transactions | Power BI Authoring MCP Server (local) | Supports Desktop and source files that the hosted endpoint cannot reach |

Use more than one MCP server when the workflow needs both remote Fabric operations and local development tooling. Keep the boundaries explicit so the agent doesn't use a broader tool surface than the task requires.

## 2. Start read-only, then expand deliberately

- Use read-only or assessment-oriented modes first when available.
- Enable write operations only after confirming the target workspace, item, tenant, and user identity.
- For Fabric Data Agent MCP workflows, publish a focused agent with a clear description because MCP clients use that description to decide when to invoke the tool.
- For Fabric IQ MCP workflows, pair the server with the Fabric IQ skill when possible and pin the public tool contract with `X-Variants: Fabric.Routing.FabricIQ.V1` when repeatability matters.
- For Fabric Data Warehouse MCP workflows, ask the agent to show and explain generated T-SQL before approving `executeSQL`.
- For Real-Time Intelligence MCP workflows, review generated KQL, monitoring thresholds, triggered actions, and playbooks before approval.
- For Power BI authoring workflows, use either hosted or local authoring for a given task, not both at once.
- Keep human confirmation prompts enabled for destructive, write, permission, and production actions.
- Avoid auto-approval flags such as skip-confirmation modes except in controlled, reviewed automation.

## 3. Apply least privilege to the identity behind the MCP server

- MCP servers don't grant extra Fabric permissions; they act within the connected user's or service principal's existing rights.
- Use Viewer/Reader-style access for discovery and assessment.
- Use Contributor/Member/Admin access only for specific workspaces and time-boxed implementation tasks.
- Prefer separate dev/test/prod identities or workspaces for automation.
- Review group-based access before asking an agent to manage permissions.

## 4. Treat local MCP servers as development tools with local machine reach

The local Fabric MCP Server can work with local files and configurations. Use it in a controlled workspace, not from arbitrary directories.

- Keep MCP configuration files out of committed repo content unless they contain no secrets and are intended as templates.
- Store tokens, client secrets, certificates, and access tokens in approved secret stores or local environment configuration.
- Limit file-system scope where the MCP host supports it.
- Close or disable MCP servers that aren't needed for the current task.

## 5. Protect customer data and metadata in agent context

MCP tools can expose workspace names, item definitions, schemas, model metadata, DAX, sample query results, and local files to the MCP host and configured LLM provider.

- Redact sensitive data before sharing sessions or prompts.
- Avoid sending PHI, PII, secrets, or regulated data to unapproved LLM providers.
- Prefer synthetic/sample data for demos and enablement.
- Review chat/session exports before sharing them with customers or partners.

## 6. Use source control and backup before write operations

- Put Fabric item definitions, PBIP/TMDL semantic models, notebooks, pipeline definitions, and deployment scripts under source control where supported.
- Back up Power BI semantic models before MCP-driven model changes.
- Use pull requests or peer review for MCP-generated changes.
- Keep a rollback path: exported item definitions, deployment pipeline history, semantic model backups, or workspace restore procedures.

## 7. Validate agent output like human-authored changes

- Validate REST responses, workspace/item state, and permissions after remote MCP operations.
- Run semantic model checks, DAX validation, refresh tests, and Best Practice Analyzer-style checks after modeling changes.
- Use Fabric deployment pipelines or Git integration for promotion, not direct unreviewed production edits.
- Document prompts or procedures that become reusable delivery patterns.

## 8. Keep release and versioning status visible

Fabric MCP server release status varies by server and workload. Record release status, version or endpoint, install method, and known limitations in delivery notes.

- Pin package or extension versions for repeatable demos.
- Re-test setup instructions before customer workshops.
- Re-check Microsoft Learn and source repos before relying on a capability in production guidance.

## 9. Pair identity-resolution MCPs with explicit permission review

Microsoft Graph MCP integration can help resolve users and groups by email address. Use it to reduce ID lookup friction, but still review the resolved principal before granting access.

- Prefer groups over individual users for durable access.
- Confirm tenant, object ID, and group purpose before applying role assignments.
- Avoid natural-language permission changes that don't name the exact workspace, principal, and role.

## 10. Keep reusable MCP patterns in this folder

Use `fabric-mcps/` for MCP server cards, setup guidance, prompt/runbook patterns, and decision guidance. Keep customer-specific implementations out of this public repository.
