# Fabric Core MCP Server - remote Fabric REST API MCP

Microsoft Learn: [Get started with Fabric Core MCP Server](https://learn.microsoft.com/en-us/rest/api/fabric/articles/mcp-servers/core-remote/get-started-core)

Endpoint: `https://api.fabric.microsoft.com/v1/mcp/core`

## What it is

Fabric Core MCP Server is the remote Microsoft Fabric MCP endpoint that exposes Fabric public REST APIs as typed MCP tools for AI agents.

Use it when an architecture or delivery scenario needs natural-language access to Fabric workspace, item, folder, or permission operations without installing a local server.

## What it covers

| Area | Why it matters |
|------|----------------|
| Remote MCP access | Connects MCP-compatible hosts to Fabric over Streamable HTTP. |
| Entra ID authentication | Uses OAuth 2.0 with Microsoft Entra ID and the connected user's Fabric permissions. |
| Workspace and item operations | Supports exploring and managing Fabric workspaces and items through Fabric REST APIs. |
| Item relations and lineage | Can use Fabric item relations APIs, where exposed, to inspect upstream and downstream item dependencies for impact analysis and lineage-aware AI. |
| Permission operations | Can inspect and manage role assignments where the connected identity is authorized. |
| Audit logging | Fabric audit logs capture actions performed through the remote server with the user's identity. |

## Best-fit scenarios

- Listing workspaces and Fabric items.
- Creating workspaces or supported Fabric items.
- Finding upstream/downstream item relations before recommending changes, deletions, or deployment promotion.
- Managing workspace role assignments.
- Giving agents typed Fabric REST API access for remote operations.
- Pairing with Microsoft Graph MCP when identity or group resolution is needed.

## Watch-outs

- Re-check Microsoft Learn for the current operation list before customer commitments; the server can expose only supported Fabric public API operations available to the connected identity.
- The item relations operations are beta and require `beta=true`; use them for evaluation, impact checks, and agent grounding until Microsoft Learn confirms production readiness.
- It doesn't bypass Fabric RBAC. The connected user can only do what their Fabric role assignments allow.
- Permission changes should name the exact workspace, principal, and role, and should be reviewed before approval.
- If the MCP host doesn't support remote Streamable HTTP servers, check host-specific guidance before using this option.

## Related Microsoft Learn references

- [Fabric Items REST API](https://learn.microsoft.com/en-us/rest/api/fabric/core/items)
- [Get upstream relations (beta)](<https://learn.microsoft.com/en-us/rest/api/fabric/core/items/get-upstream-relations(beta)>)
- [Get downstream relations (beta)](<https://learn.microsoft.com/en-us/rest/api/fabric/core/items/get-downstream-relations(beta)>)
