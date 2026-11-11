# Solutions

Public-source workshops, demos, guides, and reference implementations that apply this repository's architecture guidance to practical scenarios.

These are reference cards, not customer case studies, production certifications, or copies of upstream implementations. Follow the linked public source for setup instructions, assets, licensing, and current prerequisites. Microsoft Learn remains authoritative for product behavior.

## Browse by workload

| Category | Use for |
|----------|---------|
| Data Warehouse | Warehouse performance, SQL analytics, and capacity investigations |
| Power BI | Semantic models, connectivity choices, and analytics consumption |
| Fabric IQ | Data agents, business context, ontology, and AI grounding |
| CI/CD | Cross-workload development, deployment, and environment promotion |

Place each solution under its primary workload or cross-cutting category. List related workloads in the card rather than duplicating it. Add other workload folders when they have content.

## Solution catalog

| Solution | Primary category | Type | Important boundary |
|----------|------------------|------|--------------------|
| [Data-agent readiness and evaluation](fabric-iq/data-agent-readiness-evaluation.md) | Fabric IQ | Workshop | Evaluation SDK is preview; confirm workshop prerequisites and tenant settings |
| [Warehouse capacity investigation](data-warehouse/capacity-investigation.md) | Data Warehouse | Guide | Correlation is not causal attribution; the report can cache sensitive SQL text and identities |
| [Private-network deployment](ci-cd/private-network-deployment.md) | CI/CD | Reference implementation | Community example scoped to one standalone notebook |
| [Dev, Test, and Prod deployment](ci-cd/dev-test-prod-deployment.md) | CI/CD | Reference implementation | Community example; verify item support, environment bindings, and release gates |
| [Power BI connectivity decisions](power-bi/connectivity-decisions.md) | Power BI | Guide | Choose the storage mode against actual security, freshness, and capacity constraints |
| [IQ supply-chain demo](fabric-iq/supply-chain-demo.md) | Fabric IQ | Demo | Synthetic-data accelerator with preview dependencies; intended for evaluation |

## How to use a solution

1. Read the scenario and scope before selecting an upstream example.
2. Review the related repository guidance for the recommended architecture.
3. Check current Microsoft Learn requirements and the upstream project's prerequisites, license, and support model.
4. Evaluate in an isolated workspace with sample or approved data. Complete the card's validation checks before adapting it.

Public availability is not permission to copy code, datasets, diagrams, or decks. Keep assets upstream unless their reuse terms and publication approval have been confirmed. A successful demo does not establish production readiness.

## Adding a solution

Use the common card structure: type, primary and related workloads, public starting point, scenario, recommended approach, prerequisites and watch-outs, validation before use, related guidance, and Microsoft Learn sources.

Write an original summary grounded in public sources. Exclude customer identifiers, private links, internal evidence, and non-public product claims. Label each card as a **Workshop**, **Demo**, **Reference implementation**, or **Guide**, and make preview dependencies explicit.

## Related sections

- [Best practices](../README.md#-best-practices-17-docs) - recommended architecture principles
- [Reference architectures](../reference-architectures/patterns-comparison.md) - target designs and trade-offs
- [Toolbox](../toolbox/README.md) - tool and accelerator links
- [Contributing](../CONTRIBUTING.md) - public-content and contribution requirements
