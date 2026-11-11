---
applyTo: '**'
---
## Querying Microsoft Documentation

You have access to MCP tools called `microsoft_docs_search`, `microsoft_docs_fetch`, and `microsoft_code_sample_search` — these tools allow you to search through and fetch Microsoft's latest official documentation and code samples. That information may be more detailed or newer than what's in your training data.

When handling questions about Microsoft Fabric architecture, best practices, or anti-patterns, **start with this repo's Markdown guidance first**, then use these tools to verify product details against the latest official documentation. Specifically:

- Read the relevant local files in `best-practices/`, `anti-patterns/`, `reference-architectures/`, `fabric-data-agents/`, `security/`, and `fabric-mcps/` before relying on Microsoft Learn alone
- Use `microsoft_docs_search` to verify current Fabric docs when reviewing a design or answering architecture questions
- Use `microsoft_docs_fetch` to retrieve full articles when you need detailed implementation guidance
- Use `microsoft_code_sample_search` to find Fabric notebook/pipeline code examples
- When local repo guidance and Microsoft Learn differ, defer to Microsoft Learn for current product behavior and mention the repo may need an update

### Priority topics to search for

**Foundation & storage**
- Microsoft Fabric OneLake, shortcuts, medallion architecture
- Delta Lake format, V-Order, Delta optimization
- Lakehouse, Data Warehouse, SQL analytics endpoint
- Databases in Fabric (SQL database, Cosmos DB), OneLake replication/mirroring, open mirroring

**Ingestion, transformation & analytics**
- Data Factory: pipelines, dataflows Gen2, Copy job, CDC, connectors
- Data Engineering: Spark, notebooks, environments
- Data Science: MLflow, model registry, PREDICT, semantic link (SemPy)
- Real-Time hub: streams, Fabric events, Azure events, business events, Schema Registry
- Real-Time Intelligence: Eventhouse, Eventstream, KQL, Activator

**Modeling, BI & AI**
- Direct Lake, semantic models, star schema
- Data Agents, ontologies, and Fabric IQ
- Copilot in Fabric across workloads
- Fabric Apps (Rayfin SDK)

**Platform, governance & adoption**
- Fabric governance, domains, deployment pipelines, lifecycle management (CI/CD)
- Fabric security: OneLake security, permission model, private links
- Fabric capacity planning and workload management
- Fabric adoption and workload expansion strategy
