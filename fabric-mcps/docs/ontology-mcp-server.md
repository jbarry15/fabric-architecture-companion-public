# Ontology MCP Server

**Source:** [Use Ontology MCP Server](https://learn.microsoft.com/en-us/fabric/iq/ontology/how-to-use-ontology-mcp-server)

Endpoint: `https://api.fabric.microsoft.com/v1/mcp/dataPlane/workspaces/<workspace-id>/items/<ontology-item-id>/ontologyEndpoint`

## What it is

The Ontology MCP Server is a remote preview MCP endpoint that exposes a Fabric ontology item to external AI systems through MCP.

Use this when an AI agent needs business entities, relationships, and definitions from a Fabric ontology as part of a governed Fabric IQ workflow.

## Best fit

| Use case | Why this server fits |
|----------|----------------------|
| Business entity grounding | Exposes ontology concepts and relationships to agents |
| Fabric IQ context sharing | Makes ontology context available through the MCP protocol |
| Agent reasoning over business terms | Helps align prompts and answers to enterprise definitions |
| Ontology-centered demos | Provides a direct item-scoped endpoint for a specific ontology |

## Guardrails

- This capability is in preview; verify current tenant settings and supportability before customer delivery.
- Service principal access is not currently supported for ontology MCP.
- The tenant must allow users to create ontology preview items and Fabric items.
- Configure the endpoint from the ontology item URL and verify workspace/item IDs carefully.
- Keep ontology definitions governed; an agent can only reason as well as the business context it receives.

## References

- [Use Ontology MCP Server](https://learn.microsoft.com/en-us/fabric/iq/ontology/how-to-use-ontology-mcp-server)
- [Ontologies and Fabric IQ](../../fabric-data-agents/ontologies-fabric-iq.md)
