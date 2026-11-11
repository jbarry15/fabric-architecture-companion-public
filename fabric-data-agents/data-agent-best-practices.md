# Fabric Data Agent Best Practices

Based on [Best practices for configuring your data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-configuration-best-practices), [Fabric data agent concept](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent), and [Semantic model best practices for data agent](https://learn.microsoft.com/en-us/fabric/data-science/semantic-model-best-practices)

> **Generally available.** Fabric data agents are GA. They let users ask questions in natural language and get answers grounded in your governed Fabric data — no SQL, DAX, or KQL required.

## What a Fabric data agent is

A Fabric data agent orchestrates across **multiple governed data sources**, picks the most relevant one for each question, generates a query with the right tool, validates it, and returns a human-readable answer. It maintains **read-only** connections and enforces the caller's configured identity, workspace permissions, and Microsoft Purview governance.

### Supported data sources and query tools

| Data source | Query tool | Notes |
|-------------|-----------|-------|
| **Lakehouse** | NL2SQL | Relational tables in OneLake through the SQL analytics endpoint |
| **Warehouse** | NL2SQL | Relational; Purview DLP policies are GA here |
| **Fabric SQL Database** | NL2SQL | Operational SQL data surfaced through Fabric SQL sources |
| **Mirrored database** | NL2SQL | External operational data mirrored into Fabric for read-only analytical questions |
| **Power BI semantic model** | NL2DAX | Uses **Read** permission (Build not required for agent queries) |
| **KQL database / Eventhouse** | NL2KQL | Live + historical time-series; can use tables, materialized views, functions, and shortcuts |
| **Graph model (preview)** | GQL | Relationship-rich graph exploration |
| **Ontology (preview)** | NL2Ontology | Cross-domain shared vocabulary — see [ontologies-fabric-iq.md](ontologies-fabric-iq.md) |
| **Azure AI Search index (preview)** | Search + generation | Unstructured documents exposed through an Azure AI Search index with citations |

A single agent can combine up to five sources — for example a semantic model for KPIs plus an eventhouse for live telemetry, unified by an ontology.

## How the Data Agent works (semantic models)

```
User Question → Orchestrator → Data Source Selection → DAX Generation → Execute → Response
```

The Data Agent uses your semantic model's **schema, metadata, and Prep for AI configuration** to generate DAX queries, then executes the validated DAX against the model. This means metadata quality directly determines response accuracy.

## Prep for AI configuration (critical)

All semantic-model-specific AI configuration belongs in **Prep for AI** (Power BI), not in data agent instructions.

### 1. AI Data Schema

Define which tables, columns, and measures the AI should use:

- ✅ Select only objects relevant to the questions your agent should answer
- ✅ Include dependent objects (if a measure references other measures/columns, include those too)
- ✅ Use clear, business-friendly names (`Total Revenue` not `TR_AMT`, `Sales Region` not `DIM_GEO_01`)
- ❌ Don't include everything — more objects = more ambiguity = less accuracy

### 2. Verified Answers

Pre-approved responses for common or complex questions:

- ✅ Use 5-7 trigger questions per verified answer (cover natural variations)
- ✅ Include both formal and conversational phrasings
- ✅ Configure up to 3 filters for flexible slicing
- ❌ Don't use partial phrases as triggers — use complete questions

### 3. AI Instructions

Business context the AI wouldn't otherwise understand:

- ✅ Define business terminology ("top performer = 110%+ quota attainment")
- ✅ Specify metric preferences ("profitability = Contribution_Margin, not Gross_Profit")
- ✅ Set default groupings ("analyze revenue by fiscal quarter unless specified")
- ✅ Define time periods ("peak season = November through January")

## Implementation workflow

For a concise readiness gate before building or publishing, use the [Fabric Data Agent AI-ready checklist](ai-ready-checklist.md).

```
1. Optimize semantic model (performance, remove bloat)
       ↓
2. Define AI Data Schema (Prep for AI > Simplify data schema)
       ↓
3. Create Verified Answers (Prep for AI > common questions)
       ↓
4. Add semantic model to Data Agent (test without instructions first)
       ↓
5. Add AI Instructions (Prep for AI > based on validation findings)
       ↓
6. Prepare report visuals (clean up hidden pages/visuals)
       ↓
7. Use AI-assisted agent creation only as a bootstrap, then review the generated scope/instructions
       ↓
8. Select the standard or preview runtime deliberately
       ↓
9. Iterate based on evaluation results and user feedback
```

## Model optimization checklist

Before connecting to a Data Agent:

- [ ] Remove unnecessary columns and tables (reduce noise)
- [ ] Fix incorrect data types
- [ ] Address high-cardinality columns
- [ ] Add descriptions to tables, columns, and measures
- [ ] Use business-friendly naming throughout
- [ ] Ensure efficient DAX patterns (no circular dependencies)
- [ ] Run Best Practice Analyzer in a Fabric notebook

## Key rules

| Rule | Rationale |
|------|-----------|
| **All semantic model instructions go in Prep for AI** | The DAX generation tool ONLY reads Prep for AI config — it ignores agent-level instructions |
| **Agent-level instructions are for cross-source guidance only** | Response formatting, routing rules, tone — not model-specific logic |
| **Same tables in Data Agent and AI Data Schema** | Mismatches cause inconsistent behavior |
| **Star schema is essential** | Facts + dimensions with clear relationships produce better DAX generation |
| **Optimize before connecting** | Slow models = bad conversational UX; bloated models = reduced accuracy |

## Runtime choice

Every data agent runs on a runtime:

| Runtime | Use for |
|---------|---------|
| **Standard runtime** | Production agents that need stable, predictable query generation and routing behavior |
| **Preview runtime** | Evaluating upcoming improvements such as Advanced NL2SQL and Advanced DAX generation before they graduate to GA |

Runtime selection doesn't control which data sources or preview source types you can add. Preview runtime behavior can change more frequently, so use it deliberately, test with the same ground-truth question set, and republish the agent when changing the runtime for the published version.

For semantic models, Advanced DAX generation can improve complex question handling by planning, inspecting intermediate results, resolving ambiguous filters, and searching model values before answering. Treat it as an evaluation candidate until it is in the standard runtime for your tenant; keep production agents on the stable runtime unless the customer explicitly accepts preview behavior and test coverage.

AI-assisted agent creation, such as Build Agent with AI where available, can accelerate first-draft instructions and source configuration. Review generated scope, examples, source selection, and unsupported-use-case wording before publishing; don't let generated instructions replace domain-owner validation.

## Agent-level configuration best practices

These apply to the **agent** (cross-source routing, scope, terminology), distinct from semantic-model Prep for AI config.

- ✅ **Get data AI-ready** — use descriptive table/column names (`CustomerOrders`, `order_submission_date`), not `Table1`/`col1`/`flag`.
- ✅ **Build specialized, single-domain agents**, not one general-purpose agent for everything. A focused agent is easier to instruct and far more accurate.
- ✅ **Minimize data source scope** — connect only the sources, tables, and columns needed. Use no more than five data sources and limit to **≤25 tables** per source for best results.
- ✅ **Say what to do, not just what to avoid.** Give the correct approach and a clear fallback ("if data is missing, tell the user and recommend contacting HR").
- ✅ **Define business terms, abbreviations, and synonyms** (fiscal vs. calendar year, NPS, MAU). Put cross-source definitions in **agent-level instructions**; put dataset-specific meanings in **data source instructions**.
- ✅ **Use leading words** in data source instructions to nudge query shape (e.g., include `LIKE '%bike%'` to hint a pattern match).
- ✅ **Keep instructions concise and purposeful** — state the agent's scope, the authoritative source, fallback behavior, and tone; leave table-level specifics to data source instructions.

| Instruction level | Use for |
|-------------------|---------|
| **Agent-level instructions** | Objective, source prioritization, cross-source terminology, tone, formatting |
| **Data source instructions** | Dataset-specific term meanings, leading words, query hints for that source |
| **Prep for AI (semantic model)** | AI Data Schema, Verified Answers, AI Instructions — the DAX tool reads *only* this |

Semantic models are the exception: data source instructions and descriptions aren't supported for semantic model sources. Put semantic-model-specific guidance in Prep for AI.

## Security & governance

Data agents inherit — never bypass — your existing controls:

- **Read-only** connections to every source; no writes.
- **Least privilege** — the agent uses the caller's configured identity and permissions. Prefer user identity passthrough where each user must only reach data they're individually authorized to view.
- **Microsoft Purview** enforcement — respects **DLP policies** (GA for Fabric Data Warehouse) and **access restriction policies** (preview) for KQL Database, Fabric SQL Database, and Warehouse. Policy enforcement can block queries or withhold sensitive results.
- **Tenant settings and geography** — enable required Copilot/Azure OpenAI and cross-geo processing/storage settings where applicable, and confirm data source capacity regions are compatible.
- **Responsible AI** guardrails constrain tool invocation and outputs to scoped sources; optionally add [Azure AI Content Safety](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/overview).

## Consumption surfaces

Once built, publish the agent where users already work. Validate the authentication mode for each consumption surface and prefer user authentication/identity passthrough when user-level permissions must persist. For deeper channel-specific best practices and anti-patterns, see [Fabric Data Agent consumption patterns](consumption-patterns.md).

| Surface | Use it for | How |
|---------|-----------|-----|
| **Copilot Studio** | Add the published data agent as a Fabric IQ Data MCP tool inside a custom copilot or multi-agent orchestration; validate same-tenant setup and User vs Maker authentication | [Consume a data agent as a tool in Copilot Studio](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-microsoft-copilot-studio-tool) |
| **Microsoft Foundry** | Add published data agents through Fabric IQ / OneLake Catalog (preview); migrate the earlier Fabric tool integration, which is unsupported as of November 6, 2026 | [Foundry via Fabric IQ](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-foundry-fabric-iq) · [Retirement notice](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-foundry) |
| **MCP endpoint** | Query a published data agent from any MCP client that supports streamable HTTP and can attach a Fabric bearer token | [Data agent as MCP server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server) |
| **Python SDK / external client** | Use MCP for new runtime integrations; treat the Python client SDK sample as migration/reference guidance because its Assistants API dependency is sunset | [Consume from Python](https://learn.microsoft.com/en-us/fabric/data-science/consume-data-agent-python) |
| **Native Fabric visual responses** | Let users request generated charts directly in the Fabric data agent experience | [Visual responses](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-visuals) |
| **Sharing (in Fabric)** | Let other Fabric users query the agent directly, each with their own permissions | [Data agent sharing model](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-sharing) |

> Extending an agent outside Fabric can send responses outside Fabric's compliance boundary or geographic region depending on the client/service. Validate tenant settings, terms, data handling, and authentication mode before enabling broad consumption.

### MCP publishing guidance

When a data agent is published, Fabric exposes it as an MCP server with one tool: the agent itself. That keeps the tool surface narrow, but the agent's description becomes operationally important because MCP clients use it to decide when to invoke the tool.

- ✅ **Publish focused agents** — broad, ambiguous agents become broad, ambiguous MCP tools.
- ✅ **Write a precise description** — include the business domain, supported question types, source boundaries, and unsupported use cases.
- ✅ **Use MCP-aware clients only** — the endpoint expects the MCP initialize, tools/list, and tools/call flow; it isn't a generic REST API.
- ✅ **Review the client boundary** — responses may be processed or stored by the MCP client outside Fabric's compliance boundary or region.
- ⚠️ **Don't rely on dynamic client registration** — clients must obtain and attach a valid Fabric bearer token using their own authentication flow.

## Evaluation

- ✅ **Test without instructions first**, then add instructions based on observed gaps.
- ✅ **Systematically evaluate** answer accuracy with a question set before rollout — see [evaluate a data agent](https://learn.microsoft.com/en-us/fabric/data-science/evaluate-data-agent).
- ✅ **Iterate** on Verified Answers and instructions from real user feedback.
- ✅ **Use source control and deployment pipelines** for data agent configuration changes before production rollout — see [Data Agent ALM](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-source-control).

## Production observability

When a Fabric data agent is connected to a Microsoft Foundry agent, use Foundry observability and the attached Application Insights resource to trace the full request path: Foundry agent run, Fabric data agent call, agent span, tool spans, queried sources, duration, status, and failures. This is the production troubleshooting path for "why did the agent answer that way?", "which source did it query?", and "where did latency or failure occur?"

- ✅ **Turn on tracing before broad rollout** — connect Application Insights in the Foundry project so traces are available when users start depending on the agent.
- ✅ **Monitor tool spans, not just final answers** — latency, failures, and unexpected source selection usually show up in the data agent/tool spans before they show up as user complaints.
- ✅ **Govern telemetry deliberately** — review the tenant control for Fabric data agent logs and traces, and validate retention, access, and geography with the same care as other operational logs.
- ✅ **Pair observability with evaluation** — traces explain individual failures; ground-truth question sets measure whether changes improve the agent over time.

## Community implementation patterns

The [Fabric Data Agent Community Hub](community-resources.md) is a useful supplemental source for practitioner examples. Treat Microsoft Learn as the canonical product reference, then use community examples to identify field-tested patterns worth validating in your own tenant.

- ✅ **Separate analytical Q&A from operational actions** — use the Fabric Data Agent for governed analytical questions, and route workflow actions such as approvals, onboarding, support requests, or data issue reporting through Copilot Studio or another orchestration layer.
- ✅ **Plan for capacity impact when agents move into daily workflows** — community examples highlight that connected-agent experiences can be seamless but can also increase capacity utilization; monitor usage and size capacity before broad rollout.
- ✅ **Mirror operational data before agent grounding** — for operational SQL Server or other transactional estates, mirror into OneLake first so the agent grounds on governed analytical replicas instead of querying source systems directly.
- ✅ **Evaluate with ground-truth question sets** — manual spot checks are not enough for production. Use repeatable benchmark questions and expected answers to measure changes as the model, schema, instructions, and Verified Answers evolve.
- ✅ **Externalize meaning through semantic models and ontologies** — avoid overloading prompts with business logic. Use semantic-model metadata, Prep for AI, Verified Answers, and Fabric IQ ontologies to make meaning explicit.
- ⚠️ **Treat multilingual patterns as implementation-specific** — community examples describe translate-in/translate-out and Copilot Studio front-end patterns, but validate language behavior, risk, and supportability against current product guidance before production use.

## References

- [Semantic model best practices for data agent](https://learn.microsoft.com/en-us/fabric/data-science/semantic-model-best-practices)
- [Best practices for configuring your data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-configuration-best-practices)
- [Data agent configurations](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-configurations)
- [Add and configure data sources](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-add-datasources)
- [SQL sources in Fabric data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-sql-sources)
- [Fabric data agent concept](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent)
- [Create a data agent](https://learn.microsoft.com/en-us/fabric/data-science/how-to-create-data-agent)
- [Fabric data agent runtime](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-runtime)
- [Evaluate a data agent](https://learn.microsoft.com/en-us/fabric/data-science/evaluate-data-agent)
- [Consume a data agent as a tool in Copilot Studio](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-microsoft-copilot-studio-tool)
- [Earlier Microsoft Foundry integration retirement notice](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-foundry)
- [Consume a data agent in Microsoft Foundry via Fabric IQ](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-foundry-fabric-iq)
- [Observability for Fabric data agents in Microsoft Foundry](https://learn.microsoft.com/en-us/fabric/data-science/fabric-data-agent-foundry-observability)
- [Data agent as a Model Context Protocol server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server)
- [Fabric data agent Python SDK](https://learn.microsoft.com/en-us/fabric/data-science/fabric-data-agent-sdk)
- [Consume a data agent from external applications with Python](https://learn.microsoft.com/en-us/fabric/data-science/consume-data-agent-python)
- [Get visual responses from a Fabric data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-visuals)
- [Share a data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-sharing)
- [Source Control, CI/CD, and ALM for Fabric Data Agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-source-control)
- [Configure Fabric data agent tenant settings](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-tenant-settings)
- [Prep data for AI](https://learn.microsoft.com/en-us/power-bi/create-reports/copilot-prepare-data-ai)
- [Data Agent example queries](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-example-queries)
- [Optimize a model for performance](https://learn.microsoft.com/en-us/training/modules/optimize-model-power-bi/)
- [Power BI Modeling MCP server](https://github.com/microsoft/powerbi-modeling-mcp) — LLM-assisted renaming of model objects
- [Fabric Data Agent Community Hub](https://fabricdataagent.com/)
