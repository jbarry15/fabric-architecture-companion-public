# Microsoft Fabric Architecture Companion

An opinionated companion for teams adopting Microsoft Fabric. Contains best practices, anti-patterns, architecture comparisons, security guidance, and Copilot-powered agents and skills to help review designs — all grounded in the latest official Microsoft documentation via the Learn MCP Server.

## Who this is for

- **Solution and data architects** designing OneLake-first Fabric estates, medallion patterns, domains, and reference architectures
- **Data engineering, data science, and warehouse teams** implementing pipelines, Lakehouses, Spark, Fabric Warehouse, ML patterns, and reusable demos
- **Power BI and analytics teams** modernizing semantic models, Direct Lake architectures, star schemas, and report consumption patterns
- **AI, IQ, ontology, and data-agent builders** grounding Copilot, Fabric IQ, Foundry IQ, Work IQ, and Data Agents on governed enterprise context
- **Fabric app, MCP, and automation builders** using Fabric Apps, agent-facing tools, repo examples, and skills to operationalize reusable patterns
- **Platform, security, governance, and COE teams** setting standards for tenant administration, Purview integration, lifecycle management, capacity, quota, and cost governance
- **Customer-facing architects, consultants, and partners** applying reusable Fabric guidance to customer requirements

## Prerequisites

| Requirement | Details |
|-------------|---------|
| **Microsoft Fabric access** | A Fabric workspace (trial or paid). [Start a free trial](https://learn.microsoft.com/en-us/fabric/get-started/fabric-trial) |
| **GitHub account** | To clone and use this repo. [Create one free](https://github.com/signup) |
| **VS Code** (recommended) | With [GitHub Copilot in VS Code](https://code.visualstudio.com/docs/copilot/overview) for agent mode |
| **GitHub Copilot access** | [Free tier](https://github.com/features/copilot/plans), Pro, Business, or Enterprise |

### Optional (for enhanced experience)

| Tool | Purpose |
|------|---------|
| **GitHub Copilot CLI** | Use agents and skills from the terminal. [Install guide](https://docs.github.com/en/copilot/how-tos/copilot-cli) |
| **Power BI Desktop** | To explore Direct Lake and semantic model patterns locally |
| **Azure subscription** | If deploying Fabric capacity (F SKU) rather than using trial |

> **No coding required.** This repo is documentation and AI-powered review tools. You don't need to install Python, Node.js, or any SDKs to use it.

## Quick start

```bash
# 1. Clone the repo
git clone https://github.com/jbarry15/fabric-architecture-companion-public.git
cd fabric-architecture-companion-public

# 2. Open in VS Code (MCP server auto-configures)
code .

# 3. Start Copilot agent mode (Ctrl+Shift+I) and ask:
#    "Review my architecture against the patterns comparison"
#    "What anti-patterns should I watch for with Direct Lake?"
#    "Run the architecture checklist on my design"
```

For Copilot CLI users:
```bash
# Install the Microsoft Learn MCP plugin (one-time)
/plugin install microsoftdocs/mcp

# Then ask questions in the repo context
# Copilot will use the agents, skills, and MCP tools automatically
```

## How to use this repo

| Method | Best for |
|--------|----------|
| **Browse directly** | Reading best-practices and anti-patterns docs |
| **VS Code + Copilot agent mode** | Interactive architecture reviews with live doc lookup |
| **Copilot CLI** | Terminal-based architecture Q&A |
| **As a checklist** | Go-live readiness validation |
| **Share with teams** | Standardizing on architecture patterns across your org |

### Microsoft Learn MCP Server integration

The [Microsoft Learn MCP Server](https://learn.microsoft.com/en-us/training/support/mcp) is pre-configured in `.vscode/mcp.json`. When you open this repo in VS Code with Copilot agent mode, it automatically has access to:

- `microsoft_docs_search` — Search across all Microsoft Learn documentation
- `microsoft_docs_fetch` — Retrieve full articles for detailed guidance
- `microsoft_code_sample_search` — Find Fabric notebook/pipeline code examples

**No authentication required.** The MCP server is free and publicly available.

## What's included

### 📐 Best Practices (17 docs)
Recommended patterns with rationale, implementation guidance, and links to official docs.

### 🚫 Anti-Patterns (20 docs)
Common mistakes with clear explanation of impact, severity, and what to do instead.

### 🔒 Security (4 docs)
Security best practices, anti-patterns, and network connectivity patterns — grounded in the [Fabric Security White Paper](https://aka.ms/fabricsecuritywhitepaper).

### 🤖 Fabric Data Agents (6 docs)
How to prepare your Fabric data estate for data agents: data architecture planning, multi-source Data Agent configuration across SQL/KQL/semantic/graph/ontology/search sources, runtime and MCP consumption guidance, ontologies and **Fabric IQ** as the shared semantic layer, community resources, and agent-specific anti-patterns.

### 🧰 Fabric Skills
Curated Fabric skills and agent enablement resources for operating over Microsoft Fabric from Copilot CLI, VS Code, Claude Code, Cursor, Windsurf, and other agentic developer surfaces.

### 🔌 Fabric MCP Resources
Model Context Protocol server references, best practices, and anti-patterns for connecting AI agents to Fabric and Power BI capabilities, including Core, Fabric IQ, Data Agent, Data Warehouse, Eventhouse, Activator, Ontology, Data Factory, Real-Time Intelligence, local Fabric, and Power BI MCP servers.

### 💰 Fabric Pricing & Forecasting
Draft area for Fabric pricing, capacity planning, consumption forecasting, reservations, cost-governance guidance, best practices, and anti-patterns.

### 📏 Fabric Quota & Capacity Planning
Draft area for Fabric quota management, CU sizing, capacity operations, regional capacity planning, best practices, and anti-patterns.

### 📊 Reference Architectures
Side-by-side comparison matrix: Gold Standard ✅ vs. Silver/Transitional ⚖️ vs. Anti-Pattern 🚫 across 13 architecture dimensions. Also includes the **OneLake Architectural Guidance Framework** (strategic initiatives → proven patterns → foundational capabilities) and a Fabric data-store decision guide.

### 🤖 Copilot Agents
- **Fabric Architecture Reviewer** — Full design review against best practices
- **Anti-Pattern Detector** — Quick scan for common Fabric mistakes

### 🛠️ Copilot Skills
- **Architecture Checklist** — Scores your design against the patterns comparison matrix

## Structure

```
├── best-practices/          # Recommended patterns with rationale
│   ├── medallion-architecture.md
│   ├── onelake-shortcuts.md
│   ├── direct-lake.md
│   ├── semantic-modeling.md
│   ├── data-engineering.md
│   ├── data-science.md
│   ├── data-warehouse.md
│   ├── data-factory.md
│   ├── data-gateways.md
│   ├── mirroring.md
│   ├── real-time-hub.md
│   ├── real-time-intelligence.md
│   ├── databases-in-fabric.md
│   ├── fabric-apps.md
│   ├── copilot-in-fabric.md
│   ├── governance.md
│   └── ci-cd-deployment.md
├── anti-patterns/           # What to avoid and why
│   ├── directquery-to-external.md
│   ├── bypassing-onelake.md
│   ├── ...
│   └── inconsistent-delta-format.md
├── fabric-data-agents/      # Fabric Data Agents and agent-ready data architecture
│   ├── data-architecture-for-agents.md
│   ├── data-agent-best-practices.md
│   ├── consumption-patterns.md
│   ├── ai-ready-checklist.md
│   ├── community-resources.md
│   ├── ontologies-fabric-iq.md
│   └── anti-patterns.md
├── security/                # Security best practices and anti-patterns
│   ├── best-practices.md
│   ├── anti-patterns.md
│   └── network-security.md
├── reference-architectures/  # Gold / Silver / Anti-pattern comparison
│   ├── patterns-comparison.md
│   ├── well-architected-framework.md
│   ├── deployment-patterns.md
│   ├── end-to-end-medallion.md
│   ├── onelake-decision-framework.md
│   └── fabric-data-store-decision-guide.md
├── fabric-skills/          # Fabric skills and agent enablement resources
│   └── README.md
├── fabric-mcps/            # MCP servers and agent-facing tool resources
│   ├── README.md
│   ├── best-practices.md
│   ├── anti-patterns.md
│   ├── docs/
│   └── repos/
├── fabric-pricing-forecasting/ # Fabric pricing, forecasting, and cost governance
│   ├── README.md
│   ├── best-practices.md
│   ├── anti-patterns.md
│   └── resources/
├── fabric-quota-capacity-planning/ # Fabric quota, CU sizing, and capacity planning
│   ├── README.md
│   ├── best-practices.md
│   └── anti-patterns.md
├── .github/
│   ├── copilot-instructions.md
│   ├── agents/              # Copilot agents for architecture review
│   └── skills/              # Copilot skills for automated checks
```

## Key references

### Core Fabric documentation
- [Microsoft Fabric documentation](https://learn.microsoft.com/en-us/fabric/) — Primary source for all Fabric guidance
- [Fabric data lifecycle (end-to-end)](https://learn.microsoft.com/en-us/fabric/fundamentals/data-lifecycle)
- [Fabric Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/microsoft-fabric/overview)

### Architecture & patterns
- [Medallion Lakehouse Architecture](https://learn.microsoft.com/en-us/fabric/onelake/onelake-medallion-lakehouse-architecture)
- [OneLake Shortcuts](https://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts)
- [Mirroring in Fabric](https://learn.microsoft.com/en-us/fabric/mirroring/overview)
- [Fabric data store decision guide](https://learn.microsoft.com/en-us/fabric/fundamentals/decision-guide-data-store)
- [Real-Time hub](https://learn.microsoft.com/en-us/fabric/real-time-hub/real-time-hub-overview)
- [Direct Lake](https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview)
- [Power BI Semantic Models](https://learn.microsoft.com/en-us/power-bi/connect-data/service-datasets-understand)
- [Fabric Deployment Patterns](https://learn.microsoft.com/en-us/azure/architecture/data-guide/technology-choices/fabric-deployment-patterns)

### Governance & lifecycle
- [Fabric governance documentation](https://learn.microsoft.com/en-us/fabric/governance/)
- [Best practices for lifecycle management (CI/CD)](https://learn.microsoft.com/en-us/fabric/cicd/best-practices-cicd)
- [Best practices for domains](https://learn.microsoft.com/en-us/fabric/governance/domains-best-practices)

### Security
- [Fabric Security White Paper (PDF)](https://aka.ms/fabricsecuritywhitepaper)
- [Security in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/security/security-overview)
- [Permission Model](https://learn.microsoft.com/en-us/fabric/security/permission-model)
- [Private Links](https://learn.microsoft.com/en-us/fabric/security/security-private-links-overview)
- [Managed VNets](https://learn.microsoft.com/en-us/fabric/security/security-managed-vnets-fabric-overview)
- [Microsoft Purview integration](https://learn.microsoft.com/en-us/fabric/governance/microsoft-purview-fabric)

### AI & semantic models
- [Semantic model best practices for Data Agent](https://learn.microsoft.com/en-us/fabric/data-science/semantic-model-best-practices)
- [Data Agent example queries](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-example-queries)

### Tools
- [Fabric Toolbox (CAT)](https://github.com/microsoft/fabric-toolbox)
- [Microsoft Fabric CLI](https://github.com/microsoft/fabric-cli) — Cross-workload terminal automation for Fabric workspaces, items, jobs, OneLake files, and APIs
- [Fabric Jumpstart](https://jumpstart.fabric.microsoft.com/) — Deployable reference implementations and guided Fabric scenarios
- [Semantic Link Labs](https://github.com/microsoft/semantic-link-labs) — Notebook-based semantic model, report, lakehouse, and capacity automation utilities
- [Skills for Fabric](https://github.com/microsoft/skills-for-fabric) — Copilot/agent skill bundles for Fabric architecture, implementation, operations, migration, and Power BI authoring workflows

## Contributing

Contributions welcome! To add a new best practice or anti-pattern:

1. Create a new `.md` file in the appropriate directory
2. Follow the existing format (problem → impact → recommendation → severity → references)
3. Update `reference-architectures/patterns-comparison.md` if it adds a new architecture dimension
4. Submit a PR

For details on keeping content fresh, see [MAINTAINING.md](MAINTAINING.md).

## License

This project is provided as-is for educational and reference purposes.
