# Copilot in Fabric Best Practices

Based on [Overview of Copilot in Fabric](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-overview)

> **Release status varies by experience.** Some Fabric Copilot capabilities are GA while others remain preview. Check the [release status of AI and Copilot experiences](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-ai-feature-state) before using a capability in production, and apply [preview guidance](https://learn.microsoft.com/en-us/fabric/fundamentals/preview) where the specific experience is still preview.

## Overview

**Copilot in Fabric** is a generative-AI assistant woven through the platform. It uses large language models (Azure OpenAI Service) to help users **create, manage, and consume** Fabric items in natural language — code completion, natural-language-to-SQL/KQL, explanations, summaries, and error fixes. Each workload has its own Copilot experience, but they share a **common architecture and grounding** (the current workspace, item, and schema).

Copilot is designed to **augment, not replace** the people who build and manage Fabric items — apply it to the right problems and manage its capacity consumption.

### Copilot experiences by workload

| Workload | Current status | What Copilot does |
|----------|----------------|-------------------|
| **[Data Engineering & Data Science notebooks](https://learn.microsoft.com/en-us/fabric/data-engineering/copilot-notebooks-overview)** | Preview areas | Context-aware code completion, notebook-wide generate/refactor/summarize, performance insights, and **Fix with Copilot** / `/fix` for failed cells and Spark jobs |
| **[Data Science AI features](https://learn.microsoft.com/en-us/fabric/data-science/how-to-create-data-agent)** | Preview areas | Data agents, AI functions, AI services, and Foundry tools grounded on Fabric data |
| **[Data Factory](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-data-factory)** | GA | Generate, run, summarize, and troubleshoot pipelines; generate queries in Dataflows Gen2 |
| **[Data Warehouse](https://learn.microsoft.com/en-us/fabric/data-warehouse/copilot)** | Preview areas | Natural-language-to-SQL, code completion, quick actions, and SQL explanations |
| **[SQL database](https://learn.microsoft.com/en-us/fabric/database/sql/copilot)** | GA | NL-to-SQL, completion, quick actions, document-based Q&A, and execution-plan assistance in the portal, SSMS, and VS Code |
| **[Power BI](https://learn.microsoft.com/en-us/power-bi/create-reports/copilot-introduction)** | GA with preview subfeatures | Report page/visual suggestions, summaries, DAX assistance, Q&A enhancements, and semantic-model AI summaries where enabled |
| **[Real-Time Intelligence](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/copilot-writing-queries)** | GA | KQL query assistance, real-time dashboard generation/exploration, and operations agent capabilities |

For a broader, agent-oriented approach — grounding a conversational agent on your governed data — see [Data Agents & Fabric IQ](../fabric-data-agents/data-agent-best-practices.md).

## Platform update: Copilot becomes a Fabric IQ consumer

Recent announcements expand Copilot beyond item-local assistance toward governed enterprise context:

- **Fabric IQ in Copilot Chat and Cowork** lets users ask business questions grounded in governed Fabric/Power BI context instead of copying data into prompts.
- **Power BI data answers and Fabric IQ experiences in Microsoft 365 Copilot** expand governed semantic-model Q&A into collaboration surfaces; treat semantic-model permissions, Prep for AI, endorsements, and labels as part of Copilot readiness.
- **Data agents in Copilot Studio** are now a first-class way to reuse a Fabric data agent inside broader copilot or multi-agent workflows.
- **Published data agents as MCP servers** let MCP-aware clients ask questions through one scoped, governed agent tool.
- **Copilot for Real-Time Dashboards, Eventhouse, SQL, and Power BI app/report creation** should be treated as authoring accelerators: generated queries, reports, and actions still need review.

Architecture rule: Copilot quality follows data quality. Invest in OneLake, semantic models, ontologies, descriptions, endorsements, labels, and access policies before broad Copilot rollout.

## How it works & data handling

- Copilot experiences share a **common architecture** and are **grounded** on the item in context (workspace, notebook, attached Lakehouse schema, connected database).
- **Your table content is not sent to Azure OpenAI unless you direct it.** Data processed for Copilot includes prompts, meta-prompts, **data structure (schema)**, and conversation history — not the rows in your tables.
- Copilot runs on a **prebuilt Azure OpenAI Service** deployed in US datacenters and the EU Data Boundary. Capacities outside those regions require the admin to enable **cross-geo data processing** or Copilot is disabled by default.
- Copilot **consumes Fabric capacity** — usage draws on the same CUs as your other workloads. See [Copilot consumption](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-consumption).

## Best practices

### Enablement & governance
- ✅ **Enable deliberately via tenant settings** — turn Copilot on for the whole tenant or specific groups per your policy. See [Enable Copilot](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-enable-fabric).
- ✅ **Check SKU & region first** — Copilot needs an **F2+ or P SKU** (not trial); confirm your capacity region and set the cross-geo processing tenant setting only if your compliance posture allows it.
- ✅ **Understand the data boundary** — communicate to stakeholders that schema/prompts (not table data) are processed, and review the [privacy, security & responsible use](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-privacy-security) guidance before rollout.
- ✅ **Roll out with your COE** — pair enablement with mentoring so self-service users apply Copilot to appropriate scenarios.

### Capacity & cost
- ✅ **Monitor Copilot CU consumption** — Copilot draws on shared capacity; track usage to avoid throttling that disrupts other Fabric operations. See [consumption](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-consumption).
- ✅ **Consider a dedicated Copilot capacity** for Power BI Desktop users, or right-size to isolate AI workloads from production ETL/BI.

### Effective use
- ✅ **Ground Copilot with good structure** — clear table/column names, defined relationships, and curated [Gold-layer](medallion-architecture.md) star schemas make NL-to-SQL/KQL and report generation far more accurate.
- ✅ **Treat output as a draft** — always review generated SQL/KQL/code and report content before shipping; use the approval diff on **Fix with Copilot**.
- ✅ **Use the right surface** — notebooks for code, Warehouse/SQL for NL-to-SQL, RTI for KQL, Power BI for report authoring — rather than forcing one experience to do everything.
- ✅ **Use Fabric IQ and data agents for business Q&A** — don't ask item-local Copilot to infer enterprise definitions that belong in semantic models, ontologies, verified answers, and published data agents.

## Anti-patterns

- 🚫 **Enabling Copilot tenant-wide with no capacity monitoring** — unmanaged usage competes for CUs and can throttle production workloads.
- 🚫 **Trusting generated SQL/KQL/code blindly** — LLM output can be wrong; review and test before production use.
- 🚫 **Assuming every AI feature has the same release status** — gate preview experiences differently from GA experiences and document the production support boundary.
- 🚫 **Expecting good results over messy, undocumented data** — poor naming and no relationships degrade grounding; curate first.
- 🚫 **Assuming table data is sent to the model** — it isn't unless you direct it; equally, don't paste sensitive rows into prompts.
- 🚫 **Ignoring region/compliance settings** — enabling cross-geo processing without checking your compliance boundary can violate data-residency requirements.
- 🚫 **Treating Copilot as a replacement for skilled builders** — it augments; designs, reviews, and governance still need human ownership.

## References

- [Overview of Copilot in Fabric](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-overview)
- [Enable Copilot in Fabric](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-enable-fabric)
- [Release status of AI and Copilot experiences](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-ai-feature-state)
- [Copilot consumption (capacity)](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-consumption)
- [Privacy, security & responsible use for Copilot](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-privacy-security)
- [Copilot for Data Engineering & Data Science (notebooks)](https://learn.microsoft.com/en-us/fabric/data-engineering/copilot-notebooks-overview)
- [Copilot for Data Factory](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-data-factory)
- [Copilot for Data Warehouse](https://learn.microsoft.com/en-us/fabric/data-warehouse/copilot)
- [Copilot for SQL database](https://learn.microsoft.com/en-us/fabric/database/sql/copilot)
- [Copilot for Power BI](https://learn.microsoft.com/en-us/power-bi/create-reports/copilot-introduction)
- [Copilot for Real-Time Intelligence (KQL)](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/copilot-writing-queries)
- [Data agent in Copilot Studio](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-microsoft-copilot-studio)
- [Data agent as MCP server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server)
- [Fabric IQ overview](https://learn.microsoft.com/en-us/fabric/iq/overview)
