# Fabric Toolbox Resources

Curated links to tools, accelerators, and scripts from the [Microsoft Fabric Toolbox](https://github.com/microsoft/fabric-toolbox) (maintained by the Fabric Customer Advisory Team) and other useful resources.

## Cross-workload tools and accelerators

| Tool | Description | Link |
|------|-------------|------|
| **Microsoft Fabric CLI** | File-system-inspired CLI for navigating, scripting, automating, and operating Fabric workspaces, items, OneLake files, tables, jobs, and APIs | [GitHub](https://github.com/microsoft/fabric-cli) · [Docs](https://microsoft.github.io/fabric-cli/) |
| **Fabric Jumpstart** | Deployable reference implementations and guided scenarios for hands-on Fabric demos, workshops, and proof-of-concept starts | [Site](https://jumpstart.fabric.microsoft.com/) · [Repository reference](../solutions/fabric-other/repos/fabric-jumpstart.md) |
| **Semantic Link Labs** | Notebook-based utilities for semantic models, reports, Direct Lake migration, capacities, lakehouses, and Fabric automation | [GitHub](https://github.com/microsoft/semantic-link-labs) · [DeepWiki](https://deepwiki.com/microsoft/semantic-link-labs/1-overview) |

## Monitoring

| Tool | Description | Link |
|------|-------------|------|
| **Fabric Unified Admin Monitoring (FUAM)** | Comprehensive admin monitoring solution | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/monitoring/fabric-unified-admin-monitoring) |
| **Workspace Monitoring Dashboards** | Report templates for workspace-level monitoring | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/monitoring/workspace-monitoring-dashboards) |
| **Fabric Capacity Metrics App** | Built-in app for monitoring CU consumption | [Docs](https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app) |

## CI/CD Accelerators

| Tool | Description | Link |
|------|-------------|------|
| **Git-Based Deployments** | End-to-end CI/CD using Git integration | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/accelerators/CICD/Git-based-deployments) |
| **Fabric Deployment Pipelines** | Deploy using built-in Fabric pipelines | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/accelerators/CICD/Deploy-using-Fabric-deployment-pipelines) |
| **Branch Out to New Workspace** | Branch-per-workspace pattern | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/accelerators/CICD/Branch-out-to-new-workspace) |

## Data Warehouse

| Tool | Description | Link |
|------|-------------|------|
| **Data Warehouse Backup & Recovery** | Backup and restore patterns for Fabric DW | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/accelerators/data-warehouse-backup-and-recovery) |
| **DW Active Requests** | Monitor active queries | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/scripts/dw-active-requests) |
| **DW Queries Running at Timepoint** | Historical query analysis | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/scripts/dw-queries-running-at-timepoint) |
| **TPCH Benchmarking** | Performance benchmarking for Fabric DW | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/tools/tpch-benchmarking) |

## Migration

| Tool | Description | Link |
|------|-------------|------|
| **Power BI to Fabric DW Modernization** | Migrate Power BI datamarts to Fabric DW | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/accelerators/power-bi-to-fabric-data-warehouse-modernization) |
| **Gen2 Dedicated Pool to Fabric DW** | Table copy from Gen2 to Fabric | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/tools/Gen2toFabricDW) |
| **ADF to Fabric Data Factory** | Migration planning guide | [Docs](https://learn.microsoft.com/en-us/fabric/data-factory/migrate-planning-azure-data-factory) |

## Real-Time Intelligence

| Tool | Description | Link |
|------|-------------|------|
| **Eventhouse Accelerator** | Quick-start for Real-Time Intelligence | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/accelerators/real-time-intelligence_eventhouse) |
| **Eventstream Accelerator** | Eventstream patterns and setup | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/accelerators/real-time-intelligence_eventstream) |

## Semantic Models & Power BI

| Tool | Description | Link |
|------|-------------|------|
| **Semantic Link Labs** | Python library for advanced semantic model and lakehouse analysis in Fabric notebooks | [GitHub](https://github.com/microsoft/semantic-link-labs) · [PyPI](https://pypi.org/project/semantic-link-labs/) |
| **Best Practice Analyzer (BPA)** | Evaluates semantic models against best practices (performance, DAX, formatting, errors) | [Notebook](https://github.com/microsoft/semantic-link-labs/blob/main/notebooks/Best%20Practice%20Analyzer%20Report.ipynb) |
| **Delta Analyzer** | Analyzes Delta tables in lakehouses (row counts, data types, null counts, file stats) | [Notebook](https://github.com/microsoft/semantic-link-labs/blob/main/notebooks/Delta%20Analyzer.ipynb) |
| **Power BI Modeling MCP Server** | Local MCP server for AI-assisted semantic model authoring, DAX validation, TMDL/PBIP workflows, bulk refactoring, translations, and model documentation | [GitHub](https://github.com/microsoft/powerbi-modeling-mcp) · [Local card](../fabric-mcps/repos/powerbi-modeling-mcp.md) |
| **Semantic Model Audit** | Audit and analyze semantic model configuration | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/tools/SemanticModelAudit) |
| **DAX Performance Testing** | Benchmark DAX query performance | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/tools/DAXPerformanceTesting) |

## Administration

| Tool | Description | Link |
|------|-------------|------|
| **MicrosoftFabricMgmt PowerShell Module** | PowerShell module for Fabric management tasks | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/tools/MicrosoftFabricMgmt) |
| **Azure Policies for Capacity** | Pause or delete capacity via Azure Policy | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/samples/azure-policy) |

## Notebooks & Samples

| Tool | Description | Link |
|------|-------------|------|
| **Refresh Tables in SQL Endpoint** | Notebook for manual table refresh | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/samples/notebook-refresh-tables-in-sql-endpoint) |
| **Workspace Size** | Calculate workspace storage consumption | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/samples/notebook-workspace-size) |
| **Open Mirroring** | Mirroring implementation samples | [GitHub](https://github.com/microsoft/fabric-toolbox/tree/main/samples/open-mirroring) |

## AI & Copilot Skills

| Tool | Description | Link |
|------|-------------|------|
| **Skills for Fabric** | Reusable AI assistant instructions (skills) for GitHub Copilot CLI covering authoring, consumption, operations, migration, and architecture across all Fabric workloads | [GitHub](https://github.com/microsoft/skills-for-fabric) |

Install in Copilot CLI:

```bash
/plugin marketplace add microsoft/skills-for-fabric
/plugin install fabric-skills@fabric-collection        # full bundle
/plugin install powerbi-authoring@fabric-collection    # Power BI reports, semantic models, PBIP
```

## Additional resources

| Resource | Description | Link |
|----------|-------------|------|
| **Fabric Toolbox (full repo)** | All tools, accelerators, and scripts | [GitHub](https://github.com/microsoft/fabric-toolbox) |
| **Fabric REST APIs** | Programmatic access to Fabric resources | [Docs](https://learn.microsoft.com/en-us/rest/api/fabric/core/workspaces) |
| **Fabric Samples (Microsoft)** | Official Fabric code samples | [Repo](https://github.com/microsoft/fabric-samples) |
