# Best Practice: CI/CD and Deployment Pipelines

Based on the [Fabric CI/CD documentation](https://learn.microsoft.com/en-us/fabric/cicd/), [Plan CI/CD for Microsoft Fabric solutions](https://learn.microsoft.com/en-us/fabric/fundamentals/understand-best-practices-fabric-cicd), and [Lifecycle management best practices](https://learn.microsoft.com/en-us/fabric/cicd/best-practices-cicd)

## Overview

Treat Fabric artifacts as code. Use Git integration and deployment pipelines for version control, testing, and repeatable deployments. Modern Fabric lifecycle management combines **Git integration**, **Deployment pipelines**, **Deployment plans**, **Variable libraries**, and automation built on the **Fabric REST APIs**, **Fabric CLI**, and infrastructure-as-code tools such as Terraform and `fabric-cicd`.

## Architecture

```
Feature Branch → Pull Request → Main Branch → Dev Workspace
                                                    │
                                          Deployment Pipeline
                                                    │
                                         Test Workspace (validate)
                                                    │
                                         Production Workspace (serve)
```

## Core capabilities of Fabric CI/CD

### 1. Git integration (Continuous Integration)

Connect workspaces to a Git repository (Azure DevOps or GitHub) for version control, collaboration, and rollback.

**Supported Git providers:**
- Azure DevOps (cloud-based)
- GitHub (cloud-based)
- GitHub Enterprise (cloud-based)

**Key capabilities:**
- Workspace-level sync (including subfolder structure)
- Branch per developer or feature
- Branched workspaces for isolated feature work
- Selective branching when a developer needs only a subset of items
- Branch workspace admin profiles where administrators need governed workspace identities/settings for branch-out
- Commit, undo, and revert operations
- Conflict resolution tooling
- Granular compare (diff) for code changes
- Automation via REST APIs and service principals

### 2. Deployment pipelines (Continuous Deployment)

Automate delivery of content across dev → test → prod environments with approval gates and deployment rules. Pipelines support two to ten stages, so use more than three stages only when the release process genuinely needs them.

**Key capabilities:**
- Compare content between stages
- Deployment rules for environment-specific configs
- Deployment history and audit trail
- Automation via REST APIs (Power BI items and Fabric items)
- Impact analysis before deploying

### 3. Variable libraries

Centrally manage environment-specific values that change between stages.

**Use cases:**
- Connection strings per environment
- Item references (Lakehouse IDs, Warehouse IDs)
- Parameters that differ by stage
- Shared across multiple workspace items

### 4. Deployment plans

Use deployment plans (preview) when a Fabric deployment needs explicit item ordering or pre/post deployment actions beyond the order inferred from lineage.

**Use cases:**
- Run a notebook, pipeline, dataflow, or supported action before or after an item deploys
- Deploy dependent items in a sequence that Fabric lineage doesn't fully express
- Hydrate or validate data before promoting a Warehouse, semantic model, or downstream report
- Add repeatable deployment logic to Git integration, deployment pipelines, or supported REST API flows

Deployment plans are workspace items and should be committed with the items they reference. They are attached to a deployment operation; they are not standalone schedulers.

### 5. Automation and infrastructure as code

Use the Fabric REST APIs as the foundation for repeatable item, workspace, capacity, access, and job operations. Layer the [Fabric CLI](https://learn.microsoft.com/en-us/rest/api/fabric/articles/fabric-command-line-interface) into GitHub Actions or Azure DevOps for scriptable Git sync and deployment flows, and use the [Terraform provider for Fabric](https://registry.terraform.io/providers/microsoft/fabric/latest/docs) and [`fabric-cicd`](https://microsoft.github.io/fabric-cicd/) where teams need infrastructure-as-code or repository-driven deployments beyond the native UI.

## Supported items for Git integration

| Workload | Items |
|----------|-------|
| **Data Engineering** | Notebooks, Lakehouses, Spark Job Definitions, Environments, GraphQL, User Data Functions |
| **Data Science** | ML Experiments, ML Models, Data Agents |
| **Data Factory** | Pipelines, Dataflows Gen2, Copy Jobs, Mirrored DBs |
| **Real-Time Intelligence** | Eventhouse, EventStream, KQL Database, KQL Queryset, Real-time Dashboard, Activator |
| **Data Warehouse** | Warehouse (preview), Mirrored Databricks Catalog |
| **Power BI** | Semantic Models, Reports (PBIR), Paginated Reports, Org Apps, Metrics Sets |
| **Database** | SQL Database, Cosmos Database (preview) |

> Unsupported items in a connected workspace are ignored (not deleted) — they just can't be committed or synced.
>
> CI/CD item support changes quickly and some items remain in preview. Validate the current supported-item matrix before standardizing a workload-specific release process.

## Best practices

### Solution and environment planning

| Practice | Why |
|----------|-----|
| **Plan at the Fabric solution level** | CI/CD should cover the full set of workspace items, data containers, dependencies, and environment settings that make the solution work, not isolated artifacts one at a time |
| **Use one Git source of truth** | Multi-workspace solutions should stay in one repository and branch; use Git folder settings such as `/workspace/staging` and `/workspace/presentation` instead of splitting one solution across disconnected repos |
| **Define environments before tooling** | Decide which workspaces, capacities, data sources, connections, and approvals are required for dev, test, prod, and any UAT/PPE environments |
| **Map environment-specific values early** | Inventory URLs, paths, workspace IDs, item references, connections, and data sources so they can be parameterized instead of edited after deployment |
| **Choose the release process intentionally** | Deployment pipelines, Git synchronization, and API-driven releases solve different problems; use `fabric-cicd` or custom automation when parameterization, scale, or cross-workspace deployment needs exceed the native UI |
| **Plan post-deploy and post-sync jobs** | Git sync and deployments can create items before data, schedules, connections, value sets, and some bindings are ready; automate the follow-up work needed to make each workspace usable |

### Content preparation

| Practice | Why |
|----------|-----|
| **Separate workspaces per team** | Different teams can have different permissions, repos, branch strategies, and deployment cadences |
| **Plan your permission model** | Git repo access, pipeline stage permissions, and workspace roles should be deliberately assigned |
| **Use parameters for stage-specific configs** | Connection strings, data sources, and item references should be parameterized, not hardcoded |
| **Connect different stages to different databases** | Protect production data; don't overload it with dev/test queries |
| **Use Variable libraries first** | Centralize environment-specific values, connection references, and item references; use deployment rules or item-definition edits only where variable libraries do not cover the item type |
| **Automate repeatable operations** | Use REST APIs, Fabric CLI, Terraform, or `fabric-cicd` when UI-only deployment steps become hard to audit |

### Development stage

| Practice | Why |
|----------|-----|
| **Work in isolated environments** | Use private workspaces or client tools (VS Code, PBI Desktop) to avoid overwriting others' work |
| **Pair feature branches with feature workspaces** | Fabric authoring often requires a workspace connected to the branch being changed; use branched workspaces or a disciplined branch/workspace mapping |
| **Use selective branch workspaces deliberately** | Select only the items needed for the feature branch when a full workspace clone slows development, and include related dependencies before starting work |
| **Use branch workspace admin profiles where required** | Let admins predefine governed branch workspace settings/identities instead of relying on each developer's default workspace setup |
| **Commit to feature branches** | Never commit directly to main; use PRs with review |
| **Small, frequent commits** | Easier to merge, less conflict risk, better traceability |
| **Commit related changes together** | Items that must deploy together should be in the same commit |
| **Avoid large items in Git** | Reports with embedded images can exceed commit size limits; reduce static resources |
| **Use client tools for authoring** | VS Code for notebooks, PBI Desktop for models/reports; push to remote repo |
| **Use database project workflows for Warehouse schema** | SQL-first teams can use Visual Studio Code or SSMS database projects, DacFx-backed schema compare/build/publish workflows, and SQL formatting for reviewable Warehouse schema changes; validate current Fabric Warehouse support before making it a required production gate |
| **1:1 workspace-to-branch mapping** | One workspace connected to one branch at a time; switch branches when sprints change |
| **Keep the integration branch PR-only after seeding** | Use an initial Commit to Git to create item definitions, then merge future updates through reviewed pull requests and Update from Git |

### Test stage

| Practice | Why |
|----------|-----|
| **Simulate production environment** | Match data volume, usage volume, and capacity sizing |
| **Use separate capacity for load testing** | Avoid destabilizing production during test; separate environment capacities also improve isolation when budget and governance allow |
| **Use deployment rules for data sources** | Test against production-like data without touching prod |
| **Verify dependent items** | Changes can break downstream items; use impact analysis |
| **Validate post-sync state** | Confirm Git status, active variable library value set, item bindings, connections, schedules, and data initialization after Update from Git or deployment automation |
| **Attach deployment plans for complex releases** | Use plans when test promotion must run setup, validation, or sequencing actions that standard lineage-based order does not capture |
| **Test apps before production** | Publish app in test stage to validate end-user experience |

### Production stage

| Practice | Why |
|----------|-----|
| **Lock down access** | Limit who can deploy to production; use approval gates |
| **Never make direct changes in prod** | All changes flow through the pipeline |
| **Set deployment rules** | Override connection strings, parameters, and data sources per stage |
| **Post-deployment validation** | Run tests after deploying to verify correctness |
| **Plan for breaking changes** | Document rollback procedures; time deployments for low-impact periods |
| **Use fixed workspace membership** | Don't change prod workspace roles during deployment |
| **Use service principals for automation** | Cloud CI/CD jobs should run under managed service identities instead of individual user accounts wherever Fabric APIs support it |

### Git workflow best practices

| Practice | Details |
|----------|---------|
| **Branch strategy** | Feature branches from `main`; merge via PR with required reviewers |
| **Branch policies** | Require PR approval, successful build, and linked work items |
| **Git folder settings** | Use repository subfolders to separate workspace item definitions from pipeline scripts, or to keep multi-workspace solutions in one branch |
| **Rolling back** | Use `git revert` or `git reset` to promote older commit as HEAD; sync workspace |
| **Duplicating items** | Copy directory, change `logicalId` and display name, update dependencies |
| **Logical IDs and auto-binding** | Treat `logicalId` values as Fabric-managed metadata; don't edit them unless official guidance explicitly requires it |
| **Data items awareness** | Git stores definitions (not data); reverting may break existing data — test first |

## What to source control

| Artifact | Git? | Notes |
|----------|:---:|-------|
| Notebooks | ✅ | Full code versioning |
| Pipelines/Dataflows | ✅ | JSON definitions |
| Semantic models | ✅ | TMDL/PBIP project format where supported; default semantic models are no longer created automatically, so manage explicit semantic models |
| Reports | ✅ | PBIP/PBIR project format for source-control-friendly report definitions |
| Warehouse DDL | ✅ | Definitions/schema as code; item coverage varies by current CI/CD support |
| Lakehouses | ✅ | Structure definition (not data) |
| Environments | ✅ | Spark config and libraries |
| KQL databases/querysets | ✅ | Real-Time Intelligence items |
| Variable libraries | ✅ | Environment-specific values |
| Data content | ❌ | Never commit data to Git |
| Credentials/secrets | ❌ | Use Variable libraries or Key Vault |

## Anti-patterns

| Anti-pattern | Impact | Fix |
|---|---|---|
| **Direct-to-production changes** | No audit trail, no rollback, no testing | All changes through Git + deployment pipelines |
| **Single workspace for dev and prod** | Can't test independently, can't use deployment pipelines | Separate workspaces per stage |
| **No branch protection** | Anyone can push breaking changes to main | Require PR reviews, branch policies |
| **Hardcoded connection strings** | Breaks when deploying across stages | Use parameters or Variable libraries |
| **Committing directly to main** | No review, no isolation, merge conflicts | Feature branches + pull requests |
| **Large monolithic commits** | Hard to review, hard to rollback, may exceed size limits | Small, frequent, related commits |
| **No test stage** | Issues discovered in production | Always test with production-like data/volume |
| **Same database for all stages** | Dev queries impact production performance | Separate databases per environment |
| **Manual deployment steps** | Error-prone, inconsistent, unauditable | Automate with deployment pipelines or CI/CD tools |
| **ClickOps project setup** | Browser-only setup is hard to repeat and weak for disaster recovery | Automate workspaces, capacities, connections, Git binding, and role assignments with Terraform, Fabric CLI, or REST APIs |
| **Splitting one solution across repos** | Multi-workspace dependencies become hard to promote, parameterize, and review together | Keep one solution in one Git source of truth and use Git folder settings per workspace |
| **Ignoring breaking changes on data items** | Deployment succeeds but data is corrupted | Test definition changes on dev/test data first |
| **Everyone has pipeline deploy access** | Uncontrolled production changes | Limit deploy-to-prod to designated approvers |
| **No deployment rules** | Same config in all stages; prod uses dev data sources | Configure rules for connections per stage |
| **Skipping post-deployment validation** | Silent failures in production | Run automated tests after every production deploy |

## Network security for CI/CD

- Configure **inbound/outbound** workspace network controls for Git connectivity
- Use **service principals** for automated Git operations (not user accounts)
- Fabric supports granular workspace-level network security for CI/CD pipelines

> **Reference**: [Network security and CI/CD](https://learn.microsoft.com/en-us/fabric/cicd/cicd-security)

### CI/CD for private-link (inbound restricted) workspaces

When a workspace blocks inbound public access, the native release path changes shape. Plan for these constraints before designing the lifecycle:

| Constraint | Impact on CI/CD design |
|------------|------------------------|
| **Deployment pipelines aren't supported** with workspace inbound access protection | A workspace assigned to a deployment pipeline can't be set to deny public access, and vice versa. Release via Git plus `fabric-cicd`, Fabric CLI, or REST APIs instead |
| **Branch out is blocked** | Developers can't self-serve feature workspaces. Automate provisioning (workspace → private link service → private endpoint → DNS → Git connect) as infrastructure as code |
| **Semantic models can't exist in restricted workspaces** | The BI tier must live in non-restricted workspaces. Split the deployment into a restricted data zone and an open BI zone, scoping `item_type_in_scope` per target |
| **Git APIs honor the private network path** | CI agents must run inside the approved VNet; Microsoft-hosted agents can't reach restricted workspaces |
| **F SKU only** | Workspace-level private links aren't available on P or trial capacities |
| **One private link service per workspace** | Each environment and feature workspace consumes a PLS, a private endpoint, and a DNS record. Size against the tenant PLS limit before committing to a workspace-per-branch model |

Run deployment agents as a pool rather than a single VM: `fabric-cicd` performs a full deployment on every run without considering commit diffs, so run time scales with workspace item count rather than change size.

> **References**: [Network security and CI/CD](https://learn.microsoft.com/en-us/fabric/cicd/cicd-security) · [Supported scenarios and limitations for workspace-level private links](https://learn.microsoft.com/en-us/fabric/security/security-workspace-level-private-links-support)

## CI/CD workflow options

| Approach | When to use |
|----------|-------------|
| **Fabric deployment pipelines** | Native Fabric experience; best for teams working entirely in Fabric |
| **Deployment plans (preview)** | Explicit deployment order and pre/post deployment actions attached to Git, deployment pipeline, or supported REST API operations |
| **Git synchronization** | Branch-based releases where target workspaces can stay connected to Git and post-sync jobs handle environment-specific fixes |
| **Fabric REST APIs + Fabric CLI** | Scripted workspace, Git sync, item, and deployment operations from CI runners |
| **Azure DevOps / GitHub Actions + `fabric-cicd`** | Repository-driven deployments, advanced parameterization, dependency sequencing, and one-to-many target workspace releases |
| **Terraform provider for Fabric** | Provision workspaces, capacities, and supporting Fabric resources as infrastructure as code |
| **Local deployment** | Development on local machine; push via Git and sync through Fabric |

## Platform update: SQL agent tooling and schema review

Agent-assisted SQL tooling raises the need for disciplined schema review. Use SSMS Agent Mode, SQL Projects, Schema Compare, and SQL Formatter as accelerators for generating and understanding changes, but don't let them bypass CI/CD:

- Export or represent generated DDL in a database project or Fabric item definition.
- Compare schema before publishing and commit the reviewed delta.
- Keep generated migration scripts with the pull request when the change is nontrivial.
- Run post-deploy validation against the Warehouse, SQL database, semantic model, and downstream reports that depend on the changed schema.

## September 2026 update: deployment plans and branch governance

Recent lifecycle updates add more control for complex Fabric release processes:

- **Deployment plans (preview)** let teams define deployment groups, explicit dependencies, and supported pre/post actions. Use them when the release sequence matters or when data/setup work must run around item deployment.
- **Branch workspaces** now better support isolated feature development, including selective branching for only the items a developer needs.
- **Branch workspace admin profiles (preview)** let administrators standardize branch workspace settings and identities so feature workspaces are governed from creation.
- **PBIP / Power BI Project workflows** are increasingly important for source-control-friendly report and semantic model development; prefer text-based project formats where they fit the team's authoring process.

These capabilities strengthen the repo's guidance to treat Fabric artifacts as code, but they do not remove the need for PR review, deployment validation, rollback planning, and environment-specific configuration.

## Related solutions

- [Dev, Test, and Prod deployment](../solutions/fabric-ci-cd/dev-test-prod-deployment.md) - a public community reference for GitHub Actions and `fabric-cicd`
- [Private-network deployment](../solutions/fabric-ci-cd/private-network-deployment.md) - a scoped notebook example using a self-hosted agent and workspace-level Private Link

## References

### Core documentation
- [Application lifecycle management overview](https://learn.microsoft.com/en-us/fabric/cicd/cicd-overview)
- [Plan CI/CD for Microsoft Fabric solutions](https://learn.microsoft.com/en-us/fabric/fundamentals/understand-best-practices-fabric-cicd)
- [Lifecycle management best practices](https://learn.microsoft.com/en-us/fabric/cicd/best-practices-cicd)
- [End-to-end ALM tutorial](https://learn.microsoft.com/en-us/fabric/cicd/cicd-tutorial)
- [Fabric command-line interface](https://learn.microsoft.com/en-us/rest/api/fabric/articles/fabric-command-line-interface)

### Git integration
- [Introduction to Git integration](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration)
- [Git integration process](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-integration-process)
- [Managing branches](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/manage-branches)
- [Development process using branch workspace](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/branched-workspace)
- [Source code format](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/source-code-format)
- [Automate Git with service principals](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/automate-git-integration-with-service-principal)

### Deployment pipelines
- [Introduction to deployment pipelines](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/intro-to-deployment-pipelines)
- [Deployment rules](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/create-rules)
- [Automate deployment pipelines](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/pipeline-automation-fabric)
- [Deployment plan overview](https://learn.microsoft.com/en-us/fabric/cicd/deployment-plan/deployment-plan-overview)
- [Create a deployment plan](https://learn.microsoft.com/en-us/fabric/cicd/deployment-plan/how-to-create-deployment-plan)

### Variable libraries
- [Variable library overview](https://learn.microsoft.com/en-us/fabric/cicd/variable-library/variable-library-overview)
- [Variable libraries in CI/CD tutorial](https://learn.microsoft.com/en-us/fabric/cicd/variable-library/tutorial-variable-library)

### Tools and accelerators
- [Fabric CAT: Git-based deployments](https://github.com/microsoft/fabric-toolbox/tree/main/accelerators/CICD/Git-based-deployments)
- [fabric-cicd](https://microsoft.github.io/fabric-cicd/)
- [Terraform provider for Fabric](https://registry.terraform.io/providers/microsoft/fabric/latest/docs)
- [Network security and CI/CD](https://learn.microsoft.com/en-us/fabric/cicd/cicd-security)
- [Troubleshooting CI/CD](https://learn.microsoft.com/en-us/fabric/cicd/troubleshoot-cicd)
- [Data Warehouse development and deployment](https://learn.microsoft.com/en-us/fabric/data-warehouse/development-deployment)
- [Develop warehouse projects in Visual Studio Code](https://learn.microsoft.com/en-us/fabric/data-warehouse/develop-warehouse-project)
- [Power BI Desktop projects (PBIP)](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-overview)
- [What's new across Microsoft SQL at SQLCon/FabCon Europe 2026](https://devblogs.microsoft.com/azure-sql/whats-new-across-microsoft-sql-at-sqlcon-fabcon-europe-2026/)
