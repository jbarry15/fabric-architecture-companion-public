# Best Practice: Governance by Layer

## Overview

Effective governance in Fabric maps policies to Medallion layers and business domains. Each layer has different access patterns, ownership, and quality expectations, while domains/subdomains, OneLake catalog, Fabric policies, Purview controls, OneLake security, diagnostics, and workspace monitoring provide the platform-wide control plane.

## Governance matrix

| Aspect | Bronze | Silver | Gold |
|--------|--------|--------|------|
| **Access** | Data engineers only | Data engineers + stewards | Analysts, report builders, business |
| **Ownership** | Ingestion team | Platform / stewardship team | Domain / analytics team |
| **Quality** | Raw (as-is) | Validated, typed, deduplicated | Business-ready, certified |
| **Sensitivity** | May contain PII (masked downstream) | PII handled (masked/tokenized) | No raw PII |
| **Change control** | Automated pipelines | PR-reviewed transformations | Certified, versioned models |
| **Lineage** | Source → Bronze tracked | Bronze → Silver transformations logged | Full end-to-end lineage |

## Workspace strategy

- **Separate workspaces per layer** (or per layer + domain) — Enables independent access control
- **Naming convention:** `{team}_{domain}_{layer}` → e.g., `analytics_sales_gold`
- **Capacity alignment** — Heavy engineering workloads (Bronze/Silver) on separate capacity from BI (Gold)
- **Domain alignment** — Use Fabric domains and subdomains to delegate ownership, tenant settings, and discovery around business areas rather than tools
- **Catalog-first discovery** — Use OneLake catalog metadata, endorsements, owners, tags, and descriptions so users find governed assets instead of recreating datasets

## Key governance practices

1. **Ownership registry** — Every dataset has a named owner and steward
2. **Sensitivity labeling** — Classify data at ingestion; enforce downstream
3. **Access via groups** — Microsoft Entra groups, never individual accounts
4. **OneLake data access roles** — Use table/folder security, and row/column security where supported, especially for Viewer/item-read access patterns
5. **Purview integration** — Use sensitivity labels, DLP, and audit logs to protect and monitor regulated data; use OneLake catalog for Fabric discovery and supported lineage
6. **Item relations impact analysis** — Use the Fabric item relations API (beta) when teams need programmatic upstream/downstream dependencies for impact checks, governance inventory, CI/CD gates, or lineage-aware AI workflows
7. **OneLake diagnostics** — Enable workspace-level diagnostic logs for critical data domains so access through Fabric UI, APIs, engines, and shortcuts can be traced to a diagnostic lakehouse
8. **Fabric policies** — Use centralized policy sets where available to enforce tenant/capacity governance rules and move high-value controls toward policy-as-code
9. **Workspace outbound access protection** — For sensitive workspaces, default outbound connections to blocked and explicitly allow approved endpoints through managed private endpoints or data connection rules
10. **Workspace monitoring** — Collect supported item logs and metrics into a read-only monitoring Eventhouse when operational traceability is required
11. **Audit logging** — Enable and monitor Fabric audit logs
12. **Deployment pipelines** — No direct-to-production changes; use Git + CI/CD
13. **Data quality monitoring** — Automated checks at Silver layer boundaries

## Governance surfaces to use

| Surface | Use for |
|---------|---------|
| **Domains/subdomains** | Business ownership, delegated administration, and discoverability |
| **OneLake catalog** | Asset discovery, endorsements, owners, descriptions, tags, Explore/Govern/Secure experiences, and catalog-scoped search |
| **Catalog Search REST API (preview)** | Programmatic discovery of accessible OneLake catalog entries across workspaces for governance inventories, app search, and agent grounding |
| **Fabric policies (preview)** | Centralized governance rules, scoped policy sets, activation workflow, auditability, and policy-as-code automation |
| **OneLake security** | Table, folder, row, and column access control across supported engines |
| **OneLake diagnostics** | Data-plane access logs for "who accessed what, when, and how," including cross-workspace shortcut access; support immutable log retention for regulated scenarios |
| **Fabric item relations API (beta)** | Programmatic upstream/downstream item lineage, dependency edges, and referenced workspaces for impact analysis, governance evidence, CI/CD checks, and lineage-aware AI |
| **Workspace monitoring** | Operational logs and metrics collected into a monitoring Eventhouse for troubleshooting, dashboards, alerts, and operations-agent investigation |
| **Workspace outbound access protection** | Workspace-level control of outbound connectivity through managed private endpoints and data connection rules |
| **Purview / compliance** | Sensitivity labels, DLP, audit, and risk and compliance controls for supported Fabric workloads |
| **Deployment pipelines + Git** | Change control, review, promotion, and rollback |

Use the [current Domains REST API contract](https://learn.microsoft.com/en-us/rest/api/fabric/admin/domains/list-domains) for domain automation. The preview contract was removed in November 2026; remove preview query parameters and validate the request and response shapes before updating existing automation. This retirement does not apply to the separate item relations API.

## Transition from Purview Unified Catalog to OneLake catalog

[Microsoft is retiring Purview Unified Catalog and classic Purview data governance](https://learn.microsoft.com/en-us/purview/unified-catalog). New accounts can no longer be created, and existing capabilities remain in maintenance mode through **November 12, 2027**. The retirement does not apply to [Purview's protection and compliance features for Fabric](https://learn.microsoft.com/en-us/fabric/governance/microsoft-purview-fabric).

Use **OneLake catalog** to discover Fabric items, see who owns them, review lineage for supported items, and act on governance insights. Enforce access through the workspace, item, OneLake, or source permissions that apply to the actual query path. Keep **Microsoft Purview** for sensitivity labels, DLP, audit, insider risk, and other compliance controls where supported. Finding an item in the catalog does not grant access to its data.

Move workflows by governance outcome, not by catalog object. OneLake catalog is not a one-to-one replacement for external metadata scanning, business glossaries, critical data elements, or cross-system lineage. Keep a tool or process in place for requirements Fabric does not meet.

### Transition checklist

This repository checklist adapts the [Microsoft transition article](https://techcommunity.microsoft.com/blog/analyticsonazure/from-purview-data-governance-to-the-fabric-onelake-catalog/4573422). Saving test evidence and requiring owner sign-off are repository recommendations, not Microsoft requirements.

- [ ] **Inventory and assign each dependency.** Record the outcome, owner, downstream consumers, and disposition: move to Fabric, retain in Purview, or cover with another tool or process. Include external scans, glossary terms, critical data elements, cross-system lineage, and API consumers. Define a pass/fail criterion for each requirement before the pilot.
- [ ] **Pilot one domain and one representative asset.** Verify that an authorized user can discover and query it, and that a user without access cannot read it through the actual consumption path, such as a report, SQL endpoint, or agent. Check the lineage the workflow depends on, verify applicable sensitivity labels and expected DLP behavior, and confirm the expected activity is visible in Purview Audit. Save the results as cutover evidence.
- [ ] **Cut over in waves without interrupting protections.** Keep Purview security and compliance controls active. Update discovery links and integrations only after the domain owner and security or compliance owner sign off on the evidence. Retire each legacy scan or workflow only after its replacement passes the acceptance criteria. For contractual obligations or highly regulated workloads, [Microsoft recommends engaging your account team early](https://techcommunity.microsoft.com/blog/microsoft-security-blog/a-next-step-in-the-evolution-of-the-microsoft-data-governance-strategy/4572227).

### Microsoft Learn references for the transition

| What to verify | Microsoft Learn guidance |
|----------------|-------------------------|
| Inventory legacy dependencies | [Purview governance planning](https://learn.microsoft.com/en-us/purview/data-governance-plan) identifies the domains, collections, scans, and data products to inventory. [Classic glossary export](https://learn.microsoft.com/en-us/purview/legacy/how-to-import-export-glossary) documents how to preserve terms as CSV; it isn't a OneLake catalog import path. |
| Design the Fabric operating model | [Fabric governance in a unified data platform](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/data/fabric-governance-unified-data-platform) covers ownership and access boundaries. [Get started with Fabric governance](https://learn.microsoft.com/en-us/fabric/governance/get-started-with-fabric-governance) walks through discovery, organization, classification, and lineage. |
| Verify discovery, governance health, and lineage | [OneLake catalog overview](https://learn.microsoft.com/en-us/fabric/governance/onelake-catalog-overview) explains Explore, Govern, and Secure. [Govern your estate](https://learn.microsoft.com/en-us/fabric/governance/onelake-catalog-govern) covers insights and recommended actions. [Fabric lineage](https://learn.microsoft.com/en-us/fabric/governance/lineage) explains what the workspace view shows and when to use impact analysis. |
| Keep metadata integrations working | [Fabric metadata scanning](https://learn.microsoft.com/en-us/fabric/governance/metadata-scanning-overview) covers extracting Fabric metadata for governance tooling. For existing Purview Data Map integrations, see [same-tenant Fabric scanning](https://learn.microsoft.com/en-us/purview/register-scan-fabric-tenant) and [supported metadata and lineage](https://learn.microsoft.com/en-us/purview/data-map-lineage-fabric). These are not replacements for scanning external sources. |
| Verify access, protection, and audit | [OneLake access control](https://learn.microsoft.com/en-us/fabric/onelake/security/data-access-control-model) explains data-plane enforcement. [Information protection](https://learn.microsoft.com/en-us/fabric/governance/information-protection), [DLP for Fabric and Power BI](https://learn.microsoft.com/en-us/power-bi/enterprise/service-security-dlp-policies-for-power-bi-overview), and [Fabric activity auditing](https://learn.microsoft.com/en-us/fabric/admin/track-user-activities) help verify controls and evidence for the workloads in scope. |

## Lineage-aware impact analysis pattern

Use the Fabric item relations API (beta) when the question is "what depends on this item?" or "what does this item depend on?" and the answer needs to be automated instead of inspected manually in the portal. The API returns related Fabric items, relation edges, and referenced workspaces, so governance tools and agents can reason over actual dependencies instead of inferred naming conventions.

Best fits:

- Pre-deployment impact checks before changing a lakehouse, warehouse, semantic model, pipeline, notebook, or report.
- CI/CD gates that flag downstream consumers before destructive item or schema changes.
- Governance inventory, lineage documentation, and workspace intelligence maps.
- Lineage-aware AI assistants that need grounded upstream/downstream context before recommending changes.

Watch-outs:

- The upstream and downstream operations are beta and require `beta=true`; do not treat them as production-stable contracts without rechecking Microsoft Learn.
- Callers need read permission on the item plus `Item.Read.All` or `Item.ReadWrite.All`; validate service principal or managed identity permissions before automation.
- Treat returned relation edges as Fabric item lineage, not a replacement for data-quality checks, schema validation, sensitivity labeling, or Purview compliance workflows.

## Catalog search pattern

Use the OneLake Catalog Search REST API when a governance tool, app, or agent needs to discover Fabric items across workspaces without scraping workspace lists or relying on naming conventions. Search results are metadata entries only and are filtered to what the calling user, service principal, or managed identity is authorized to discover.

Best fits:

- Governance inventory and catalog enrichment.
- Search experiences in internal portals or Fabric Apps.
- Agent grounding where the agent needs to locate candidate Fabric items before using a narrower API or MCP tool.
- Data product discovery across domains and workspaces.

Watch-outs:

- Catalog Search is preview and requires `Catalog.Read.All`; validate tenant approval before using it in production automation.
- A returned catalog entry does not grant data or item-content access. Use item-specific APIs, permissions, and approvals for follow-on operations.
- Keep owner, endorsement, description, and domain metadata healthy; search quality follows metadata quality.

## Diagnostic logging pattern

For regulated or high-value domains, route OneLake diagnostics into a governed lakehouse in the same capacity, transform the JSON events into queryable Delta tables, and expose usage, access, and exception dashboards to data owners and security teams. Treat diagnostic logs as their own governed data product: define retention, consider immutable logs for compliance, and decide whether the tenant should include or redact end-user identifiers such as email and IP address.

## Platform update: from catalog governance to policy and observability

Recent Fabric governance announcements push the platform toward centralized policy management and deeper operational visibility:

- **Fabric policies (preview)** centralize governance rules through policy sets that can be created, activated, audited, and automated through REST APIs. Use them for controls that should be enforced consistently across tenant or capacity scopes, but keep preview status visible.
- **Workspace monitoring / Observability Insights** sends supported logs and metrics to a monitoring Eventhouse with a read-only KQL database. Use it when data owners or operations teams need item-level troubleshooting beyond audit logs.
- **Operations agents** can investigate anomalies and root-cause patterns from Fabric-native telemetry. Treat their recommendations as governed operational evidence, not unmanaged automation.
- **Outbound access protection** is becoming an important workspace boundary control for OneLake, Data Factory, mirrored databases, Eventstream, and operations-agent actions.

## Anti-patterns it prevents

- [Tight coupling](../anti-patterns/tight-coupling.md) — Clear layer boundaries enable team independence
- [No Silver layer](../anti-patterns/no-silver-layer.md) — Governance requires a validated intermediate layer

## References

- [Fabric governance overview](https://learn.microsoft.com/en-us/fabric/governance/governance-compliance-overview)
- [Fabric domains](https://learn.microsoft.com/en-us/fabric/governance/domains)
- [OneLake catalog](https://learn.microsoft.com/en-us/fabric/governance/onelake-catalog-overview)
- [Catalog Search REST API](https://learn.microsoft.com/en-us/rest/api/fabric/core/catalog/search)
- [OneLake security access control model](https://learn.microsoft.com/en-us/fabric/onelake/security/data-access-control-model)
- [OneLake diagnostics](https://learn.microsoft.com/en-us/fabric/onelake/onelake-diagnostics-overview)
- [Fabric policies overview](https://learn.microsoft.com/en-us/fabric/governance/fabric-policies-overview)
- [Automate Fabric policy sets with policy-as-code](https://learn.microsoft.com/en-us/fabric/governance/fabric-policies-rest-api)
- [Fabric Items REST API](https://learn.microsoft.com/en-us/rest/api/fabric/core/items)
- [Get upstream relations (beta)](<https://learn.microsoft.com/en-us/rest/api/fabric/core/items/get-upstream-relations(beta)>)
- [Get downstream relations (beta)](<https://learn.microsoft.com/en-us/rest/api/fabric/core/items/get-downstream-relations(beta)>)
- [Workspace monitoring overview](https://learn.microsoft.com/en-us/fabric/fundamentals/workspace-monitoring-overview)
- [Workspace outbound access protection overview](https://learn.microsoft.com/en-us/fabric/security/workspace-outbound-access-protection-overview)
- [OneLake tenant settings](https://learn.microsoft.com/en-us/fabric/admin/service-admin-portal-onelake)
- [Fabric Well-Architected: Security](https://learn.microsoft.com/en-us/azure/well-architected/microsoft-fabric/)
