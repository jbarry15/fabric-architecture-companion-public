# Solutions

Public-source workshops, demos, guides, and reference implementations that apply this repository's architecture guidance to practical scenarios.

These are reference cards, not customer case studies, production certifications, or copies of upstream implementations. Follow the linked public source for setup instructions, assets, licensing, and current prerequisites. Microsoft Learn remains authoritative for product behavior.

## Browse by workload

| Category | Use for |
|----------|---------|
| [Data Engineering and Data Science](data-engineering-data-science/) | Lakehouse engineering, Spark, notebooks, and machine learning |
| [Data Integration](data-integration/) | Ingestion, orchestration, data movement, and presentation references |
| [Data Warehouse](data-warehouse/) | Warehouse performance, SQL analytics, capacity investigations, and repository references |
| [Fabric Administration, Governance, and Security](fabric-admin-governance-security/) | Platform operations, governance, capacity, identity, and security |
| [Fabric AI, IQ, and Ontology](fabric-ai-iq-ontology/) | Data agents, business context, ontology, and AI grounding |
| [Fabric Apps](fabric-apps/) | Application experiences and application integration |
| [Fabric CI/CD](fabric-ci-cd/) | Cross-workload development, deployment, and environment promotion |
| [Fabric Databases](fabric-databases/) | Operational databases, migrations, and database-to-analytics integration |
| [Other Fabric Solutions](fabric-other/) | Cross-workload scenarios without a more specific category |
| [Fabric and Purview Integration](fabric-purview-integration/) | Cataloging, discovery, and governance integration |
| [Power BI](power-bi/) | Semantic models, connectivity choices, and analytics consumption |
| [Real-Time Intelligence and IQ](real-time-intelligence-iq/) | Streaming ingestion, event-driven analytics, KQL, and business context |

Place each solution under its primary workload or cross-cutting category. List related workloads in the card rather than duplicating it. These are navigation categories, not an official product-workload taxonomy.

The catalog contains 20 entries: two scenario and decision guides and 18 repository references. Repository-reference cards live in their workload's `repos/` folder; original scenario and decision guides stay at the workload root. Categories without cataloged entries have starter READMEs with related guidance, not additional implemented solutions. Public implementation and presentation assets stay upstream.

## Solution catalog

| Solution | Primary category | Type | Important boundary |
|----------|------------------|------|--------------------|
| [Warehouse capacity investigation](data-warehouse/capacity-investigation.md) | Data Warehouse | Guide | Correlation is not causal attribution; the report can cache sensitive SQL text and identities |
| [Power BI connectivity decisions](power-bi/connectivity-decisions.md) | Power BI | Guide | Choose the storage mode against actual security, freshness, and capacity constraints |

## Repository references

| Reference | Primary category | Type | Important boundary |
|-----------|------------------|------|--------------------|
| [Data-agent readiness and evaluation](fabric-ai-iq-ontology/repos/data-agent-readiness-evaluation.md) | Fabric AI, IQ, and Ontology | Workshop | Evaluation SDK is preview; confirm workshop prerequisites and tenant settings |
| [Private-network deployment](fabric-ci-cd/repos/private-network-deployment.md) | Fabric CI/CD | Reference implementation | Community example scoped to one standalone notebook |
| [Dev, Test, and Prod deployment](fabric-ci-cd/repos/dev-test-prod-deployment.md) | Fabric CI/CD | Reference implementation | Community example; verify item support, environment bindings, and release gates |
| [FabricCICD runbook series](fabric-ci-cd/repos/fabriccicd-runbooks.md) | Fabric CI/CD | Guide | Community series of 25 runbooks; verify current product support and environment requirements |
| [fabric-cicd Python deployment library](fabric-ci-cd/repos/fabric-cicd.md) | Fabric CI/CD | Guide | Actual publishing library, distinct from the community FabricCICD runbook series |
| [FabricSecurity runbooks](fabric-admin-governance-security/repos/fabric-security-runbooks.md) | Fabric Administration, Governance, and Security | Guide | Community procedures; do not treat undocumented behavior as confirmed support |
| [Fabric Atlas](fabric-admin-governance-security/repos/fabric-atlas.md) | Fabric Administration, Governance, and Security | Reference implementation | Shared metadata and agent visibility require a deliberately restricted audience |
| [FabricAPI](fabric-admin-governance-security/repos/fabric-api.md) | Fabric Administration, Governance, and Security | Reference implementation | Fabric, Power BI, and Graph audit workflows have separate authorization requirements |
| [Terraform provider for Fabric](fabric-admin-governance-security/repos/terraform-provider-fabric.md) | Fabric Administration, Governance, and Security | Guide | Upstream explicitly excludes normal Microsoft/Fabric support coverage |
| [Synapse to Fabric migration utilities](data-warehouse/repos/fabric-migration.md) | Data Warehouse | Reference implementation | SQL and Spark migration aids; cross-linked from Data Engineering rather than duplicated |
| [Fabric Jumpstart](fabric-other/repos/fabric-jumpstart.md) | Other Fabric Solutions | Reference implementation | Installer and catalog website are one project; review writes, overwrite behavior, and cleanup |
| [AI-assisted Tableau migration](power-bi/repos/tableau-to-powerbi-migration.md) | Power BI | Reference implementation | Generated output requires independent numeric, functional, and security validation |
| [Power BI Ops Agent](power-bi/repos/pbi-ops-agent.md) | Power BI | Demo | Proof of concept; diagnostic recommendations are not automatic remediation |
| [IQ supply-chain demo](fabric-ai-iq-ontology/repos/supply-chain-demo.md) | Fabric AI, IQ, and Ontology | Demo | Synthetic-data accelerator with preview dependencies; intended for evaluation |
| [Fabric IQ and Copilot finance demo](fabric-ai-iq-ontology/repos/fabric-iq-copilot-demo.md) | Fabric AI, IQ, and Ontology | Demo | Public demo materials are not a complete setup guide |
| [Microsoft Rayfin](fabric-apps/repos/microsoft-rayfin.md) | Fabric Apps | Guide | Fabric Apps is preview; local frontend development can deploy a Fabric backend |
| [Rayfin Onboarding 2026](fabric-apps/repos/rayfin-onboarding-2026.md) | Fabric Apps | Workshop | Use public Fabric setup guidance and verify the separate data-agent prerequisites |
| [Operations IQ](fabric-apps/repos/operations-iq.md) | Fabric Apps | Reference implementation | Fabric Apps preview; validate multi-service permissions and action approval |

## How to use a solution

1. Read the scenario and scope before selecting an upstream example.
2. Review the related repository guidance for the recommended architecture.
3. Check current Microsoft Learn requirements and the upstream project's prerequisites, license, and support model.
4. Evaluate in an isolated workspace with sample or approved data. Complete the card's validation checks before adapting it.

Public availability is not permission to copy code, datasets, diagrams, or decks. Keep assets upstream unless their reuse terms and publication approval have been confirmed. A successful demo does not establish production readiness.

## Adding a solution

Use the common card structure: type, primary and related workloads, public starting point, scenario, recommended approach, prerequisites and watch-outs, validation before use, related guidance, and Microsoft Learn sources.

Put cards centered on an external repository, including repository-backed workshops and guide series, in `<category>/repos/`. Keep original scenario and decision guides in the category root: a supplementary repository link or repository-hosted paper does not by itself make a guide a repository reference. Use `presentations/` for standalone presentation references where applicable.

List each card once in this catalog, keep category indexes and relative links current, and leave implementation code and presentation assets upstream.

Write an original summary grounded in public sources. Exclude customer identifiers, private links, and non-public material. Label each card as a **Workshop**, **Demo**, **Reference implementation**, or **Guide**, and make preview dependencies explicit.

## Related sections

- [Best practices](../README.md#-best-practices-17-docs) - recommended architecture principles
- [Reference architectures](../reference-architectures/patterns-comparison.md) - target designs and trade-offs
- [Toolbox](../toolbox/README.md) - tool and accelerator links
