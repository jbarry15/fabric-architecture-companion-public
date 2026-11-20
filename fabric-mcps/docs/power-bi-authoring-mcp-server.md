# Power BI Authoring MCP Server

**Source:** [Power BI Authoring MCP server](https://learn.microsoft.com/en-us/power-bi/developer/mcp/power-bi-authoring-mcp)

Hosted endpoint: `https://api.fabric.microsoft.com/v1/mcp/powerbi/authoring`

Local source: [microsoft/powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp)

## What it is

The Power BI Authoring MCP server gives AI agents tools to create and change Power BI semantic models.

It has two deployment options:

- **Hosted preview** - Microsoft-hosted Streamable HTTP endpoint for semantic models in Fabric workspaces.
- **Local generally available** - local binaries over `stdio` for Fabric workspace models, Power BI Desktop, and PBIP/TMDL files on disk.

## Best fit

| Use case | Prefer |
|----------|--------|
| Hosted semantic model authoring in Fabric workspaces | Hosted Power BI Authoring MCP Server |
| Power BI Desktop model editing | Local Power BI Authoring MCP Server |
| PBIP or TMDL file workflows on disk | Local Power BI Authoring MCP Server |
| CI or service-principal-friendly modeling workflows | Local Power BI Authoring MCP Server |
| Bulk model refactoring, DAX validation, translations, or security role changes | Either hosted or local, depending on target location |

## Guardrails

- Do not register hosted and local authoring servers at the same time for the same workflow; overlapping toolsets make routing ambiguous.
- Use hosted authoring for Fabric workspace semantic models when the tenant setting is enabled and local file/Desktop access is not required.
- Use the local deployment when work requires Power BI Desktop, PBIP, TMDL files, transactions, traces, or service-principal authentication.
- Back up or source-control semantic models before agent-driven authoring changes.
- Pair the tools with Power BI/Fabric skills so the agent has modeling guidance, not just write access.

## References

- [Power BI Authoring MCP server](https://learn.microsoft.com/en-us/power-bi/developer/mcp/power-bi-authoring-mcp)
- [Power BI Authoring MCP Server (local)](../repos/powerbi-modeling-mcp.md)
- [Hosted modeling setup in Skills for Fabric](https://github.com/microsoft/skills-for-fabric/blob/main/mcp-setup/README.md)
