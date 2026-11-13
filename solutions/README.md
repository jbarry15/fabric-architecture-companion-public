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

The catalog contains nine entries: seven scenario cards and two Fabric Apps repository references. Categories without cataloged entries have starter READMEs with related guidance, not additional implemented solutions. The `presentations/` and `repos/` subfolders index public references while keeping assets upstream.

## Solution catalog

| Solution | Primary category | Type | Important boundary |
|----------|------------------|------|--------------------|
| [Data-agent readiness and evaluation](fabric-ai-iq-ontology/data-agent-readiness-evaluation.md) | Fabric AI, IQ, and Ontology | Workshop | Evaluation SDK is preview; confirm workshop prerequisites and tenant settings |
| [Warehouse capacity investigation](data-warehouse/capacity-investigation.md) | Data Warehouse | Guide | Correlation is not causal attribution; the report can cache sensitive SQL text and identities |
| [Private-network deployment](fabric-ci-cd/private-network-deployment.md) | Fabric CI/CD | Reference implementation | Community example scoped to one standalone notebook |
| [Dev, Test, and Prod deployment](fabric-ci-cd/dev-test-prod-deployment.md) | Fabric CI/CD | Reference implementation | Community example; verify item support, environment bindings, and release gates |
| [FabricCICD runbook series](fabric-ci-cd/fabriccicd-runbooks.md) | Fabric CI/CD | Guide | Community series of 25 runbooks; verify current product support and environment requirements |
| [Power BI connectivity decisions](power-bi/connectivity-decisions.md) | Power BI | Guide | Choose the storage mode against actual security, freshness, and capacity constraints |
| [IQ supply-chain demo](fabric-ai-iq-ontology/supply-chain-demo.md) | Fabric AI, IQ, and Ontology | Demo | Synthetic-data accelerator with preview dependencies; intended for evaluation |

## Fabric Apps repository references

| Reference | Type | Important boundary |
|-----------|------|--------------------|
| [Microsoft Rayfin](fabric-apps/repos/microsoft-rayfin.md) | Guide | Fabric Apps is preview; local frontend development can deploy a Fabric backend |
| [Rayfin Onboarding 2026](fabric-apps/repos/rayfin-onboarding-2026.md) | Workshop | Adapt event-specific setup; validate separate data-agent prerequisites |

## How to use a solution

1. Read the scenario and scope before selecting an upstream example.
2. Review the related repository guidance for the recommended architecture.
3. Check current Microsoft Learn requirements and the upstream project's prerequisites, license, and support model.
4. Evaluate in an isolated workspace with sample or approved data. Complete the card's validation checks before adapting it.

Public availability is not permission to copy code, datasets, diagrams, or decks. Keep assets upstream unless their reuse terms and publication approval have been confirmed. A successful demo does not establish production readiness.

## Adding a solution

Use the common card structure: type, primary and related workloads, public starting point, scenario, recommended approach, prerequisites and watch-outs, validation before use, related guidance, and Microsoft Learn sources.

Put solution cards in the category root. Where present, use `presentations/` and `repos/` for supporting public-source references, not duplicate solution cards or copied upstream assets.

Write an original summary grounded in public sources. Exclude customer identifiers, private links, internal evidence, and non-public product claims. Label each card as a **Workshop**, **Demo**, **Reference implementation**, or **Guide**, and make preview dependencies explicit.

## Related sections

- [Best practices](../README.md#-best-practices-17-docs) - recommended architecture principles
- [Reference architectures](../reference-architectures/patterns-comparison.md) - target designs and trade-offs
- [Toolbox](../toolbox/README.md) - tool and accelerator links
- [Contributing](../CONTRIBUTING.md) - public-content and contribution requirements
