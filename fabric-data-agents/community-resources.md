# Community Resources for Fabric Data Agents

The [Fabric Data Agent Community Hub](https://fabricdataagent.com/) is a community-maintained catalog of articles, videos, tools, events, learning paths, and LinkedIn posts for Microsoft Fabric Data Agents.

Use this page as a curated pointer to community resources and implementation patterns. Microsoft Learn remains the source of truth for product behavior, limits, tenant settings, security, governance, and supported configuration.

## How to use the hub

| Hub area | Use it for |
|----------|------------|
| [Articles](https://fabricdataagent.com/articles/) | Practitioner writeups, implementation lessons, field architecture patterns, and evaluation approaches |
| [Videos](https://fabricdataagent.com/videos/) | Walkthroughs, demos, setup guidance, Copilot Studio integration examples, and architecture discussions |
| [Resources](https://fabricdataagent.com/resources/) | Helpful reference material and supporting content for Data Agent builders |
| [Tools](https://fabricdataagent.com/tools/) | Utilities and examples that support Data Agent development, testing, and operations |
| [Learning paths](https://fabricdataagent.com/learn/) | Training-oriented content for getting started and building repeatable skills |
| [Contribute](https://fabricdataagent.com/contribute/) | Contribution path for adding new community content to the hub |

## Reusable implementation patterns from the hub

The hub content reinforces several repeatable implementation patterns that fit this repo's Fabric guidance.

| Pattern | How to apply it | Example community reference |
|---------|-----------------|-----------------------------|
| Ground agents on mirrored operational data | Mirror SQL Server or other operational sources into OneLake before exposing them to Data Agents. This preserves operational system ownership while enabling governed natural-language analytics over analytical replicas. | [Building a Fabric Data Agent with On-Premises SQL Server Data](https://www.linkedin.com/pulse/building-fabric-data-agent-onpremises-sql-server-pablo-junco-boquer-1xv8e/) |
| Use Copilot Studio for orchestration, not data access expansion | Let Copilot Studio handle conversation, channel, and workflow orchestration while the Fabric Data Agent handles governed analytical Q&A with configured authentication and permission boundaries. | [How to connect a Fabric Data Agent to Microsoft Copilot Studio as a connected agent](https://www.linkedin.com/pulse/how-connect-fabric-data-agent-microsoft-copilot-manoj-annavajjala-viwlc/) |
| Separate analytical questions from operational actions | Route analytics to the Fabric Data Agent and route approvals, onboarding, support, or issue reporting to operational flows. This avoids forcing one agent to mix read-only data Q&A with transactional process execution. | [Fabric Data Agent Community Hub](https://fabricdataagent.com/) |
| Evaluate before go-live | Build repeatable ground-truth question sets and expected answers so agent quality can be measured over time instead of relying on manual spot checks. | [Ground Truth Before Go-Live: Building Better Fabric Data Agents with Automated Evaluation Datasets](https://community.fabric.microsoft.com/t5/Data-Science-Community-Blog/Ground-Truth-Before-Go-Live-Building-Better-Fabric-Data-Agents/ba-p/5303092) |
| Make business meaning explicit | Use semantic models, Prep for AI, Verified Answers, and Fabric IQ ontologies so agents navigate defined business concepts instead of inferring meaning from ambiguous schema names. | [Beyond the Star Schema: Why Ontologies are the New Requirement for AI-Ready Data](https://www.linkedin.com/pulse/beyond-star-schema-why-ontologies-new-requirement-ai-ready-tony-smith-yp37e) |
| Automate semantic-model preparation where possible | Use tooling and metadata-driven approaches to keep semantic model synonyms, descriptions, and Prep for AI configuration aligned with business language. | [Automatically populate Data Agents with Semantic Model Synonyms](https://data-marc.com/2025/06/04/automatically-populate-data-agents-with-semantic-model-synonyms/) |
| Include Eventhouse/KQL scenarios where they fit | For telemetry, observability, or time-series use cases, validate Data Agent support for Eventhouse tables, functions, materialized views, and shortcut tables against current product guidance. | [Data Agent Now Supports Eventhouse Functions, Materialized Views, and Shortcuts](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/Data-Agent-Now-Supports-Eventhouse-Functions-Materialized-Views/ba-p/5181801) |
| Use the published data agent MCP endpoint for runtime integration | When custom apps or other agents need to query a published Fabric data agent, prefer the MCP endpoint and authenticate with a Fabric bearer token rather than building long-term integrations on older sample clients. | [Data agent as Model Context Protocol server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server) |
| Treat multilingual scenarios as designed patterns | For non-English users, test language behavior deliberately and document any translation, Copilot Studio, or API-based pattern used to support the experience. | [Using Microsoft Fabric Data Agent in Non-English Languages: A Practical Exploration](https://medium.com/data-science-collective/using-microsoft-fabric-data-agent-in-non-english-languages-a-practical-exploration-bf1dcb8e4c02) |

## Recommended validation before reuse

Before adopting a community pattern into production guidance, validate:

1. **Current product support** — Confirm the pattern still matches current Microsoft Learn guidance.
2. **Security and governance** — Verify identity passthrough, Purview policy behavior, data sensitivity, and access boundaries.
3. **Capacity impact** — Estimate and monitor capacity usage, especially when exposing agents through Copilot Studio or broad user channels.
4. **Source freshness** — Confirm the article, video, or tool is still maintained and compatible with the current Fabric experience.
5. **Owner and support path** — Identify who will maintain any adopted artifact or implementation guidance.

## Official references

- [Fabric data agent concept](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent)
- [Best practices for configuring your data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-configuration-best-practices)
- [Semantic model best practices for data agent](https://learn.microsoft.com/en-us/fabric/data-science/semantic-model-best-practices)
- [Evaluate a data agent](https://learn.microsoft.com/en-us/fabric/data-science/evaluate-data-agent)
- [Consume a data agent in Copilot Studio](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-microsoft-copilot-studio)
- [Data agent as Model Context Protocol server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server)
- [Fabric data agent Python SDK](https://learn.microsoft.com/en-us/fabric/data-science/fabric-data-agent-sdk)
- [Consume a data agent from external applications with Python](https://learn.microsoft.com/en-us/fabric/data-science/consume-data-agent-python)
- [Fabric data agent runtime](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-runtime)
- [Fabric Data Agent Community Hub](https://fabricdataagent.com/)
