# Activator MCP Server

**Source:** [Get started with the Activator remote MCP server](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-remote-activator)

Endpoint: `https://api.fabric.microsoft.com/v1/mcp/workspaces/<workspace-id>/reflexes/<artifact-id>`

## What it is

The Activator MCP server is a remote preview MCP endpoint that lets AI agents interact with a Fabric Activator item through natural language.

Use this when an agent needs to create or manage monitoring rules, alerts, and Teams or email actions against supported KQL data sources.

## Best fit

| Use case | Why this server fits |
|----------|----------------------|
| Monitoring rule creation | `create_rule` can create rules that watch streams and trigger actions |
| Alert lifecycle management | `list_rules`, `start_rule`, and `stop_rule` support basic rule operations |
| KQL-backed operational alerts | Supports Fabric Eventhouse and Azure Data Explorer/Kusto sources |
| Teams or email notification scenarios | Exposes action creation for Teams messages and email |

## Guardrails

- Configure a separate MCP server entry per Activator artifact.
- Include the exact workspace ID and Activator artifact ID in prompts when creating or managing rules.
- Start with nonproduction streams or low-risk rules, then validate triggered actions.
- Confirm recipients, thresholds, occurrence windows, and grouping columns before approving rule creation.
- Current limitations include KQL data sources only, one eventstream per rule, Teams/email actions only, and no multi-event trigger correlation.

## References

- [Get started with the Activator remote MCP server](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-remote-activator)
- [What is MCP in Real-Time Intelligence?](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-overview)
