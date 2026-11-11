# Fabric MCP Anti-Patterns

Common mistakes when introducing MCP servers and agent-facing tooling into Microsoft Fabric architecture and delivery workflows.

## 1. Using one MCP server for every task

### Anti-pattern

Connecting every available MCP server and letting the agent choose without boundaries.

### Why it's risky

- Expands the tool surface beyond what the task requires.
- Makes it harder to predict whether the agent will use remote Fabric APIs, local file access, semantic model tooling, or identity tooling.
- Increases review burden and accidental-change risk.

### Better pattern

Connect only the MCP servers needed for the job: Fabric Core MCP Server for remote workspace/item/permission operations, Fabric IQ MCP Server for read-only Power BI report and semantic model exploration, Fabric Data Agent MCP Server for governed Q&A through a published agent, Fabric Data Warehouse MCP Server for approved T-SQL against Warehouse or SQL analytics endpoint context, Eventhouse or Real-Time Intelligence MCP servers for KQL and RTI workflows, Activator or Operations Agent MCP servers for monitoring actions, Ontology MCP Server for Fabric IQ ontology context, DataFactory MCP Server for pipelines and Airflow jobs, local Fabric MCP Server for development and OneLake/API-spec workflows, and Power BI authoring/modeling MCP servers for semantic modeling.

## 2. Treating RBAC enforcement as a substitute for least privilege

### Anti-pattern

Using an Admin, Member, broad Contributor, or overprivileged service principal because the MCP server "respects Fabric permissions."

### Why it's risky

RBAC enforcement prevents privilege escalation, but it doesn't prevent mistakes inside the permissions the identity already has. An agent using a broad identity can still create, modify, delete, or share items across the identity's authorized scope.

### Better pattern

Use the narrowest identity and workspace role that can complete the task. Prefer scoped dev/test workspaces, temporary elevation, and group-based permission management.

## 3. Disabling confirmations for write or production actions

### Anti-pattern

Running MCP servers with auto-approval or skip-confirmation behavior against production workspaces or semantic models.

### Why it's risky

Natural-language intent can be ambiguous. Without confirmation gates, an agent may apply a valid but unintended model, workspace, permission, or item change.

### Better pattern

Keep confirmation prompts enabled for writes, destructive operations, permission changes, and production targets. Use read-only mode for assessment where available.

## 4. Letting MCP agents operate without source control or backups

### Anti-pattern

Using MCP tooling to directly modify semantic models, item definitions, notebooks, or workspace assets with no export, backup, Git history, or rollback path.

### Why it's risky

MCP-driven changes can be fast and broad. If the output is wrong, undocumented, or partially applied, recovery depends on prior backups and deployment history.

### Better pattern

Back up semantic models, use PBIP/TMDL and Fabric Git integration where supported, review generated diffs, and promote through deployment pipelines instead of direct production edits.

## 5. Sending sensitive data or metadata to unapproved LLM providers

### Anti-pattern

Assuming MCP traffic is safe because the server authenticates to Fabric, then exposing schemas, sample data, DAX, workspace names, customer identifiers, or local files in prompts and chat logs.

### Why it's risky

MCP servers can retrieve sensitive metadata and data, and the MCP host may include that context in model prompts, responses, logs, or shared chat sessions.

### Better pattern

Use approved AI environments, redact sensitive content, avoid regulated data in demos, and review session artifacts before sharing.

## 6. Running local MCP servers from an uncontrolled folder

### Anti-pattern

Starting a local Fabric MCP server or semantic modeling MCP server from a broad local directory that contains unrelated customer files, credentials, exports, or work-in-progress content.

### Why it's risky

Local MCP servers and MCP hosts may expose local file context to the agent. A broad working directory increases the chance of accidental reads, writes, or prompt leakage.

### Better pattern

Run local MCP workflows from a dedicated project folder. Keep secrets outside the repo, avoid broad file-system permissions, and close MCP tools that aren't needed.

## 7. Treating preview MCP capabilities as production commitments

### Anti-pattern

Positioning preview MCP servers, APIs, tool names, or prompts as stable production architecture without checking current Microsoft Learn and source repo guidance.

### Why it's risky

Preview features and MCP tool schemas can change before general availability. Setup flows, supported operations, and limitations may move quickly.

### Better pattern

Mark preview dependencies explicitly, pin versions for workshops, validate setup before delivery, and avoid hard production commitments until supportability is clear.

## 8. Granting permissions from natural language without identity verification

### Anti-pattern

Asking an agent to "add John as Contributor" or "give Marketing access" and approving the change without validating the resolved Entra principal.

### Why it's risky

Names and aliases can be ambiguous across tenants, guests, groups, and service principals. The agent may resolve a different object than intended.

### Better pattern

Use Microsoft Graph MCP or Entra lookup to resolve identities, then verify tenant, object ID, group purpose, workspace, and role before approving permission changes.

## 9. Committing live MCP configuration or credentials

### Anti-pattern

Checking `.vscode/mcp.json`, token values, client secrets, certificates, or local executable paths into the repo as if they were reusable guidance.

### Why it's risky

MCP configs can reveal internal paths, endpoints, tenant assumptions, credentials, or unsupported local setup details.

### Better pattern

Commit sanitized templates only. Use placeholders for tenant-specific values and keep secrets in approved secret stores or local environment configuration.

## 10. Skipping validation because the agent reported success

### Anti-pattern

Accepting an agent's natural-language success response without checking Fabric state, semantic model behavior, permissions, refreshes, or deployment results.

### Why it's risky

The tool call may have partially succeeded, updated a different object, or produced a syntactically valid but semantically wrong result.

### Better pattern

Verify the expected state directly. For semantic models, validate DAX and refresh behavior. For Fabric items, confirm item definitions, permissions, lineage, and deployment promotion results.
