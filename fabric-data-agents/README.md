# Fabric Data Agents

How to prepare your Fabric data estate for Fabric data agents, ensure they operate on governed data, and avoid common pitfalls.

## Overview

Fabric data agents are a generally available Fabric capability for building conversational Q&A over governed Fabric data. Their accuracy depends on the quality, accessibility, scope, and governance of the underlying sources. A well-architected Fabric platform is the foundation for successful agent adoption.

This section covers:
- [Data architecture for Fabric Data Agents](data-architecture-for-agents.md) — Preparing your data estate
- [Fabric Data Agent AI-ready checklist](ai-ready-checklist.md) — Readiness checks before building or publishing an agent
- [Fabric Data Agent best practices](data-agent-best-practices.md) — Multi-source agents, configuration, security, and consumption
- [Fabric Data Agent consumption patterns](consumption-patterns.md) — Best practices and anti-patterns for Copilot Studio, Foundry, MCP, custom apps, and visuals
- [Ontologies & Fabric IQ](ontologies-fabric-iq.md) — The shared semantic layer for humans and agents
- [Community resources for Fabric Data Agents](community-resources.md) — Practitioner articles, videos, tools, and reusable implementation patterns
- [Anti-patterns for Fabric Data Agents](anti-patterns.md) — What to avoid

Current Fabric data agent scenarios include SQL sources (Lakehouse, Warehouse, Fabric SQL Database, mirrored databases), Eventhouse KQL databases, Power BI semantic models, Graph models, ontologies, and Azure AI Search indexes. Published data agents can also be consumed as MCP servers by Copilot Studio, VS Code, or custom MCP clients, making the agent a reusable governed tool for Fabric IQ-grounded Q&A. Treat preview source types, preview runtime features, SDK automation, and MCP endpoint consumption as capabilities to validate before production use.

## Fabric Data Agent adoption framework

Based on [Microsoft's AI Agent Adoption Process](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/data-architecture-plan):

```
Plan for Agents → Govern & Secure → Build Agents → Manage & Integrate
      │
      ├── Business plan
      ├── Technology plan
      ├── Organizational readiness
      └── Data architecture  ← THIS SECTION
```

## Key principle

> **Unified data platform first, agents second.** Don't build agents on fragmented data. Establish OneLake as the governed foundation, then layer AI capabilities on top.

## Platform update: agents as reusable governed tools

Recent Fabric announcements reinforce the same architecture: data agents should sit on governed OneLake data, semantic models, and Fabric IQ ontologies, then be exposed through the user experiences where people and agents already work. Keep the reusable pattern simple:

1. Ground the agent on a scoped domain model and governed Fabric sources.
2. Publish the agent with a clear description, source boundaries, and owner.
3. Reuse it through supported consumption surfaces such as Copilot Studio, MCP clients, Microsoft Foundry, or custom clients.
4. Monitor usage, answer quality, and data-source selection before broad rollout.

## Related solutions

- [Data-agent readiness and evaluation](../solutions/fabric-ai-iq-ontology/repos/data-agent-readiness-evaluation.md) - a public Microsoft workshop for measurable answer-quality improvements
- [IQ supply-chain demo](../solutions/fabric-ai-iq-ontology/repos/supply-chain-demo.md) - a synthetic-data accelerator with explicit preview dependencies

## References

- [Data architecture plan for AI agents (CAF)](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/data-architecture-plan)
- [What is Fabric IQ?](https://learn.microsoft.com/en-us/fabric/iq/overview)
- [What is Ontology (preview)?](https://learn.microsoft.com/en-us/fabric/iq/ontology/overview)
- [Fabric data agent concept](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent)
- [Add data sources to a data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-add-datasources)
- [Fabric Data Agent semantic model best practices](https://learn.microsoft.com/en-us/fabric/data-science/semantic-model-best-practices)
- [Fabric data agent runtime](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-runtime)
- [Data agent as MCP server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server)
- [Data Agent example queries](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-example-queries)
- [Fabric Data Agent Community Hub](https://fabricdataagent.com/)
