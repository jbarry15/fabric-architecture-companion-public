# Anti-Patterns: Fabric Data Agents

Common mistakes when building Fabric Data Agents that consume Fabric data.

---

## 🚫 Building agents on fragmented data

### What it looks like
- Agent connected to multiple ungoverned sources with overlapping/conflicting data
- No unified data platform; agent pulls from ADLS, Databricks, legacy SQL, and SharePoint simultaneously
- No clear "source of truth" for key business entities

### Why it's a problem
- Agents synthesize information — conflicting sources produce unreliable/contradictory answers
- No governance means no way to audit what the agent is using
- Security gaps when access policies aren't consistent across sources

### What to do instead
✅ Unify on OneLake first. Build Medallion layers. Certify Gold data products. Then connect agents.

---

## 🚫 Putting semantic model instructions in the Data Agent

### What it looks like
- Business terminology, metric definitions, and DAX guidance added at the agent level
- Assuming agent-level instructions will guide DAX query generation

### Why it's a problem
- **The DAX generation tool ignores agent-level instructions entirely.** It only reads Prep for AI configuration on the semantic model itself.
- Results in agents that seem configured correctly but produce wrong answers

### What to do instead
✅ All semantic-model-specific instructions go in **Prep for AI** (AI Instructions tab). Agent-level instructions are only for cross-source routing, tone, and formatting.

---

## 🚫 Exposing the full semantic model to the agent

### What it looks like
- All tables, columns, and measures selected when adding semantic model to Data Agent
- No AI Data Schema configured in Prep for AI
- 200+ columns available for the AI to choose from

### Why it's a problem
- More objects = more ambiguity = lower accuracy
- Similarly-named fields across tables cause incorrect query generation
- Slower response times (more metadata to parse)

### What to do instead
✅ Configure AI Data Schema in Prep for AI. Select only the objects relevant to your agent's scope. Ensure Data Agent table selection matches.

---

## 🚫 Using poorly optimized semantic models

### What it looks like
- Large models with unnecessary columns, incorrect data types, high cardinality
- Complex/circular DAX measures
- No descriptions on tables, columns, or measures
- Technical naming (`DIM_GEO_01`, `TR_AMT`, `FK_CUST_ID`)

### Why it's a problem
- Slow query execution = bad conversational UX (users expect sub-second responses)
- No descriptions means the AI can't understand object purpose
- Technical names mean the AI can't map natural language to the correct fields
- Bloated models reduce accuracy of DAX generation

### What to do instead
✅ Run Best Practice Analyzer. Remove unnecessary objects. Add descriptions. Use business-friendly naming. Optimize before connecting to Data Agent.

---

## 🚫 No verified answers for common questions

### What it looks like
- Users repeatedly ask the same questions and get inconsistent or wrong answers
- Ambiguous terms (e.g., "sales", "performance", "territory") resolved differently each time
- No pre-approved responses for high-value/high-frequency questions

### Why it's a problem
- The AI must guess intent every time, with no guidance
- Inconsistent answers erode user trust
- Complex multi-step questions often fail without guidance

### What to do instead
✅ Identify top 20-30 common questions. Create Verified Answers in Prep for AI with 5-7 trigger variations each. Cover formal and conversational phrasings.

---

## 🚫 Skipping the unified data platform step

### What it looks like
- Jumping straight to agent building without data architecture planning
- No Medallion layers, no data quality enforcement
- Agents reading from Bronze/raw data directly

### Why it's a problem
- Raw data has no quality guarantees → agents return incorrect information
- No governance → can't audit or control what agents access
- No semantic layer → agents can't understand business context

### What to do instead
✅ Follow the adoption sequence: Unify data platform → Govern & secure → Build agents → Manage & integrate. See [data-architecture-for-agents.md](data-architecture-for-agents.md).

---

## 🚫 MCP servers without authentication

### What it looks like
- MCP tool endpoints accessible without identity verification
- No audit trail for tool invocations
- Shared service accounts instead of identity passthrough

### Why it's a problem
- No accountability for agent actions (who triggered what?)
- Security violation — agents could access data beyond user permissions
- Compliance risk — no audit trail for regulatory requirements

### What to do instead
✅ Require authentication on every MCP tool call. Use Entra identity passthrough when user-level permissions must persist. Enable detailed auditing. For published Fabric data agents consumed as MCP servers, attach a valid Fabric bearer token and verify the caller has access to the workspace and data agent.

---

## 🚫 Redefining business concepts per agent instead of a shared ontology

### What it looks like
- Each agent (and each semantic model) defines "Customer", "Shipment", or "Breach" its own way
- Cross-domain questions require hand-written joins baked into agent instructions
- No shared, governed vocabulary spanning lakehouse, eventhouse, and semantic models

### Why it's a problem
- Definitions drift → different agents give different answers to the same question
- No single place to govern meaning, units, or relationships
- Cross-domain reasoning is brittle and duplicated across every agent

### What to do instead
✅ Define concepts **once** in an [ontology (preview)](ontologies-fabric-iq.md), bind them to OneLake sources, and let agents consume that shared layer. Generate the ontology from existing semantic models to keep terminology aligned.

---

## 🚫 Connecting too many sources and tables to one agent

### What it looks like
- An entire lakehouse or model connected with all tables and columns
- One general-purpose agent expected to answer every question across all domains
- More than five data sources attached to one agent
- Far more than ~25 tables exposed per data source

### Why it's a problem
- More objects = more ambiguity = lower accuracy and slower responses
- The agent struggles to route to the right source
- Hard to instruct, test, and govern

### What to do instead
✅ Build **specialized, single-domain agents**. Connect only the sources needed, stay within the five-source limit, and keep to **≤25 tables per source**. Use agent-level instructions for routing and terminology.

---

## 🚫 Treating direct files as data agent sources

### What it looks like
- Expecting the data agent to answer directly from PDFs, DOCX files, text files, or raw lakehouse files
- Adding CSV/JSON files to OneLake but not exposing them through tables
- Treating an Azure AI Search unstructured-data source as production-ready without checking preview constraints

### Why it's a problem
- Fabric data agents don't directly query standalone lakehouse files
- Unstructured retrieval requires a supported Azure AI Search index and is preview
- Users get incomplete answers when files aren't exposed through supported tables or indexes

### What to do instead
✅ Ingest files into governed tables, expose them through a supported SQL/semantic/KQL source, or use an Azure AI Search index for unstructured content after validating preview support and security boundaries.

---

## 🚫 Using the old Assistants API path as a long-term runtime integration

### What it looks like
- Building new external apps on the OpenAI Assistants API sample path
- Treating the Fabric data agent Python SDK as both the management plane and the permanent runtime query surface
- Ignoring the published data agent MCP endpoint

### Why it's a problem
- Microsoft Learn states the Python client sample uses the OpenAI Assistants API path and should be migrated to the MCP endpoint
- The old Assistants API path has a stated August 26, 2026 shutdown date and is no longer a durable runtime integration target
- Query/runtime code can become obsolete even if management-plane SDK code remains valid

### What to do instead
✅ Use the Python SDK for management-plane tasks such as create, configure, update, and publish. For runtime querying, plan around the published data agent MCP endpoint and validate supported clients, authentication, and migration timing.

---

## 🚫 Assuming all consumption surfaces preserve the same boundary

### What it looks like
- Publishing a data agent into Copilot Studio, Foundry, an external app, or an MCP client without reviewing data handling
- Assuming every surface always runs as the requesting user
- Ignoring cross-geo processing/storage settings and client terms

### Why it's a problem
- Some non-Fabric services can process or store returned responses outside Fabric's compliance boundary or geographic region
- Copilot Studio supports different authentication choices for connected agents, so the selected mode matters
- Data access can be correct while response handling still violates customer policy

### What to do instead
✅ Validate tenant settings, geographic processing/storage requirements, client data handling terms, and authentication mode before exposing a data agent outside Fabric. Prefer user authentication or identity passthrough when user-level data permissions must persist.

---

## 🚫 Using preview runtime in production without a test harness

### What it looks like
- Enabling preview runtime for a production agent because Advanced NL2SQL or Advanced DAX generation looks better in a demo
- Republishing without comparing answers against a ground-truth question set
- Not recording which runtime a published agent uses

### Why it's a problem
- Preview runtime receives more frequent changes to orchestration, routing, and query-generation tools
- Behavior can change before features graduate to GA
- A good demo answer doesn't prove production stability

### What to do instead
✅ Use standard runtime for production stability. Use preview runtime to test upcoming improvements, then compare results against the same evaluation set before republishing.

---

## 🚫 Reasoning over a stale ontology graph

### What it looks like
- Upstream tables get new rows, but the ontology graph is never refreshed
- Agents answer using out-of-date entity instances
- No defined refresh cadence for the ontology item

### Why it's a problem
- The ontology graph does **not** auto-reflect upstream changes — a manual refresh is required
- Agents confidently return stale or incomplete results
- Erodes trust in an otherwise governed layer

### What to do instead
✅ Establish a **refresh cadence** aligned to how fast source data changes, and refresh the graph model before agents rely on it for time-sensitive questions.

---

## Summary table

| Anti-Pattern | Severity | Key fix |
|-------------|:--------:|---------|
| Building on fragmented data | 🚫 High | Unify on OneLake first |
| Instructions at agent level | 🚫 High | Use Prep for AI instead |
| Full model exposed to agent | ⚠️ Medium-High | Configure AI Data Schema |
| Poorly optimized models | ⚠️ Medium | Run Best Practice Analyzer |
| No verified answers | ⚠️ Medium | Create for top 20-30 questions |
| Skipping unified platform | 🚫 High | Follow adoption sequence |
| MCP without authentication | 🚫 High | Entra identity + audit logging |
| Concept redefined per agent | 🚫 High | Define once in a shared ontology |
| Too many sources/tables per agent | ⚠️ Medium | Specialized agents, ≤5 sources, ≤25 tables/source |
| Direct files as agent sources | ⚠️ Medium | Expose files through tables or Azure AI Search |
| Old Assistants API runtime integration | ⚠️ Medium | Use MCP endpoint for published-agent runtime queries |
| Assuming all surfaces preserve boundary | 🚫 High | Validate auth mode and response data handling |
| Preview runtime without test harness | ⚠️ Medium | Evaluate before republishing |
| Stale ontology graph | ⚠️ Medium | Set a refresh cadence |

## References

- [Semantic model best practices for data agent](https://learn.microsoft.com/en-us/fabric/data-science/semantic-model-best-practices)
- [Best practices for configuring your data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-configuration-best-practices)
- [Add data sources to a data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-add-datasources)
- [Fabric data agent runtime](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-runtime)
- [Data agent as a Model Context Protocol server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server)
- [Fabric data agent Python SDK](https://learn.microsoft.com/en-us/fabric/data-science/fabric-data-agent-sdk)
- [Consume a data agent from external applications with Python](https://learn.microsoft.com/en-us/fabric/data-science/consume-data-agent-python)
- [Fabric Data Agent consumption patterns](consumption-patterns.md)
- [What is Ontology (preview)?](https://learn.microsoft.com/en-us/fabric/iq/ontology/overview)
- [Data architecture plan for AI agents (CAF)](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/data-architecture-plan)
- [MCP authentication support](https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/mcp-authentication)
