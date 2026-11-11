# Operations Agent MCP Server

**Source:** [What is MCP in Real-Time Intelligence?](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-overview)

## What it is

The Operations agent MCP server is a remote Real-Time Intelligence MCP server for configuring and operating Fabric operations agents through natural language.

Use this when an AI agent needs to configure operations agent instructions, generate playbooks, or manage monitoring workflows for operational data.

## Best fit

| Use case | Why this server fits |
|----------|----------------------|
| Operations-agent setup | Helps configure instructions for how the agent monitors and responds |
| Playbook generation | Can generate operational playbooks from real-time monitoring context |
| Monitoring management | Supports natural-language management of operations-agent monitoring workflows |
| Real-Time Intelligence operations | Fits scenarios where Eventhouse, Activator, and operations agents are part of one operational loop |

## Guardrails

- Treat generated playbooks as operational changes that require review.
- Validate monitored assets, thresholds, actions, and escalation paths before approving changes.
- Use least-privilege Fabric roles for operations-agent management.
- Review MCP client and model-provider data handling before exposing production operational telemetry.
- Re-check Microsoft Learn before customer delivery because Real-Time Intelligence MCP capabilities are evolving.

## References

- [What is MCP in Real-Time Intelligence?](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/mcp-overview)
