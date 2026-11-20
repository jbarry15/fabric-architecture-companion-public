# Fabric Security Best Practices

## 1. Identity & Access Management

### Use Entra ID Conditional Access

- Enforce MFA for all Fabric users, especially admins
- Restrict access by location, device compliance, and sign-in risk
- Block legacy authentication protocols
- Use session controls to limit data exfiltration

> **Reference**: [Conditional Access in Fabric](https://learn.microsoft.com/en-us/fabric/security/security-conditional-access)

### Apply Least-Privilege Workspace Roles

| Role | Use When |
|------|----------|
| Viewer | Consuming reports and dashboards only |
| Contributor | Building items but not managing access |
| Member | Managing items and sharing within team |
| Admin | Workspace administration only (minimize count) |

- Prefer Entra security groups over individual assignments
- Review workspace membership quarterly
- Use item-level sharing for cross-team access instead of elevating workspace roles

### Manage Service Principals Carefully

- Use service principals (not user accounts) for automation and CI/CD
- Scope service principal access to specific workspaces
- Rotate secrets/certificates regularly
- Audit service principal activity

## 2. Network Security

### Implement Private Links for Sensitive Workloads

- Use **workspace-level Private Link** when only selected workspaces require private inbound access
- Use **tenant-level Private Link** only when the whole tenant must be reachable only through private endpoints
- Block public internet access only after validating all supported and unsupported access paths
- Test thoroughly — tenant-level private links affect all workspaces in the tenant; workspace-level private links have their own DNS/FQDN and private endpoint planning

> **References**: [Private Links in Fabric](https://learn.microsoft.com/en-us/fabric/security/security-private-links-overview) · [Workspace-level Private Links](https://learn.microsoft.com/en-us/fabric/security/security-workspace-level-private-links-overview) · [Protect inbound traffic](https://learn.microsoft.com/en-us/fabric/security/protect-inbound-traffic)

### Use Managed VNets and Managed Private Endpoints

- Enable managed VNets for supported workloads accessing sensitive external sources
- Create managed private endpoints to reach supported private sources and destinations such as Azure SQL, ADLS Gen2, and Eventstream sources
- Avoid opening public endpoints on data sources just for Fabric access
- Validate supported item types and limitations first: managed private endpoints primarily support Spark-based Data Engineering/Data Science workloads; Eventstream managed private endpoints are GA for Azure Event Hubs and Azure IoT Hub; OneLake shortcuts to ADLS Gen2 and Azure Blob don't yet support managed private endpoints

> **References**: [Managed Private Endpoints](https://learn.microsoft.com/en-us/fabric/security/security-managed-private-endpoints-overview) · [Eventstream managed private endpoints](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/set-up-private-endpoint)

### Leverage Trusted Workspace Access

- For ADLS Gen2 with firewall rules, use workspace identity + trusted access
- Prefer resource instance rules for specific Fabric workspaces over the broader trusted service exception
- Avoid IP allowlisting Fabric IPs when trusted workspace access is available
- Requires a purchased F SKU capacity and workspace identity; Trial capacity isn't supported

### Secure Outbound Connectivity

- Use on-premises data gateway for on-prem sources (keep gateway updated)
- Use VNet data gateway for Azure-resident sources behind VNets
- Prefer service tags over broad IP allowlisting
- For sensitive Fabric workspaces, use **workspace outbound access protection** to block outbound connections by default and explicitly allow approved endpoints through managed private endpoints or data connection rules
- Validate feature-specific outbound access support: OneLake/Data Engineering use managed private endpoints, Data Factory and mirrored databases use data connection rules, and operations-agent outbound actions have preview limitations

## 3. Data Protection

### Encrypt with Customer-Managed Keys (CMK)

- Use [workspace CMK](https://learn.microsoft.com/en-us/fabric/security/workspace-customer-managed-keys) for highly sensitive data
- Store keys in Azure Key Vault with soft-delete and purge protection enabled
- Define key rotation policy (90-day recommended)
- Plan for key unavailability scenarios

### Implement Row, Column, and Object-Level Security

- **RLS**: Filter data rows per user identity (SQL and DAX-based)
- **CLS**: Hide sensitive columns from unauthorized users
- **OLS**: Hide entire tables or measures from the semantic model
- Test security rules with "View As" functionality
- Document which roles map to which data access

> **Reference**: [Row-Level Security](https://learn.microsoft.com/en-us/fabric/security/service-admin-row-level-security)

### Configure Direct Lake with Security

- Understand the mode difference: Direct Lake on OneLake enforces OneLake access directly and doesn't apply SQL endpoint RLS; Direct Lake on SQL can fall back to DirectQuery when SQL endpoint RLS or unsupported features require it and fallback is enabled
- Use fixed identity cloud connections for Direct Lake semantic models with semantic-model RLS to avoid per-user token overhead and inconsistent performance
- Test both security correctness and performance at production scale; don't assume a Direct Lake model and a SQL endpoint enforce identical policies

### Apply OneLake Security (Data-Plane, "Secure Once, Use Everywhere")

OneLake security is the **data-plane** security model: define granular, role-based access to data at the OneLake layer and have it enforced consistently across supported engines such as Lakehouse, Spark notebooks, SQL analytics endpoint in user's identity access mode, Direct Lake on OneLake, and authorized external engines. This is distinct from semantic-model RLS above, which protects the model — not every underlying data access path.

- **Understand control plane vs. data plane** — Workspace roles and item permissions govern *what users can manage*; OneLake security governs *what data users can read*, regardless of management rights
- **Define OneLake security roles** with their current components: **type** (grant), **permission** (Read or ReadWrite), **scope** (tables, folders, or schemas), and **members** (Entra users, groups, or nonuser identities)
- **Secure at the right level** — OneLake security currently supports Lakehouse Read/ReadWrite and read permissions for supported mirrored catalogs/databases; don't assume every Fabric item has identical role support
- **Grant least privilege to readers** — OneLake security roles primarily protect Viewer/item-read access paths; Admin/Member/Contributor workspace roles grant elevated OneLake access and override read restrictions, so keep those roles small
- **Review default roles** — Lakehouses and supported mirrored items can create `DefaultReader` roles that grant broad read access to users with ReadAll; edit or remove default roles when broad read access isn't intended
- **Apply table/folder, column, and row security where supported** — Lakehouse, Spark notebooks, SQL analytics endpoint in user's identity mode, Direct Lake on OneLake, and Eventhouse support RLS/CLS filtering. Authorized third-party engine integration is preview and requires the engine to enforce the policies; check [engine-specific requirements](https://learn.microsoft.com/en-us/fabric/onelake/security/read-secured-data)
- **Secure shortcuts deliberately** — Shortcut access is governed by the most restrictive combination of shortcut-path and target-path permissions, with exceptions for Direct Lake on SQL and delegated T-SQL paths that use the item owner's identity to reach the target and then apply OneLake roles
- **Enforce across authorized external engines** — Authorized third-party engines can retrieve OneLake policy definitions and enforce table permissions, RLS, and CLS at query time; OneLake stays the source of truth while the engine enforces policy
- **Authenticate with Entra ID** — OneLake maps Entra user/service-principal identities to the permissions you set; enable SPNs at the tenant level for automation

> **References**: [OneLake data security overview](https://learn.microsoft.com/en-us/fabric/onelake/security/get-started-security) · [Data access control model](https://learn.microsoft.com/en-us/fabric/onelake/security/data-access-control-model) · [Row-level security in OneLake](https://learn.microsoft.com/en-us/fabric/onelake/security/row-level-security) · [Column-level security in OneLake](https://learn.microsoft.com/en-us/fabric/onelake/security/column-level-security) · [Best practices for securing data in OneLake](https://learn.microsoft.com/en-us/fabric/onelake/security/best-practices-secure-data-in-onelake)

### Restrict AI and Agent Access Deliberately

- Use sensitivity labels, DLP policies, OneLake security, and access restriction policies before exposing governed data to Copilot, data agents, MCP clients, or operations agents
- For operations agents in OAP-protected workspaces, keep action targets in the same workspace during preview and validate Teams, Power Automate, and cross-workspace action behavior before enabling automation
- For published data-agent MCP endpoints, review the MCP client data boundary because responses may be processed or stored outside Fabric's compliance boundary or region

> **References**: [Workspace outbound access protection](https://learn.microsoft.com/en-us/fabric/security/workspace-outbound-access-protection-overview) · [Workspace outbound access protection for OneLake](https://learn.microsoft.com/en-us/fabric/security/workspace-outbound-access-protection-onelake) · [Outbound Access Protection for Operations Agent](https://learn.microsoft.com/en-us/fabric/security/workspace-outbound-access-protection-operations-agent) · [Data agent as MCP server](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-mcp-server)

## 4. Governance & Compliance

**Microsoft Purview protections still apply to supported Fabric workloads.** The retirement of Purview Unified Catalog and classic data governance does not retire Information Protection, Data Loss Prevention (DLP), Audit, Insider Risk Management, or supported AI compliance controls. Use [OneLake catalog for Fabric discovery and governance](../best-practices/governance.md#transition-from-purview-unified-catalog-to-onelake-catalog), and [Microsoft Purview for protection, audit, and compliance](https://learn.microsoft.com/en-us/fabric/governance/microsoft-purview-fabric). Verify licensing, workload coverage, and policy behavior before relying on a control.

### Apply Sensitivity Labels from Microsoft Purview

- Define a labeling taxonomy aligned with your data classification policy
- Enable mandatory labeling for Fabric items
- Labels follow data through Fabric (source → transform → report)
- Combine with DLP policies to prevent sensitive data leakage

> **Reference**: [Information Protection in Fabric](https://learn.microsoft.com/en-us/fabric/governance/information-protection)

### Use Purview Audit and OneLake Diagnostics

- Use [Microsoft Purview Audit](https://learn.microsoft.com/en-us/fabric/admin/track-user-activities) to investigate Fabric user and admin activity. Forward relevant events to your SIEM if needed; verify the event types and retention your investigation requires.
- Enable **OneLake diagnostics** where data-plane visibility matters; it streams workspace-level data access events into a lakehouse so teams can answer "who accessed what, when, and how" across Fabric UI, API, engine, and shortcut access paths
- Use immutable OneLake diagnostic logs when audit records must be tamper-resistant, and align cleanup jobs with the immutability and retention windows
- Review the tenant setting for end-user identifiers in OneLake diagnostic logs so email/IP capture matches privacy and investigation requirements
- Monitor: workspace access changes, data exports, sharing events, admin actions
- Set alerts for anomalous patterns (bulk downloads, off-hours access)
- Retain logs per regulatory requirements (minimum 90 days recommended)

> **References**: [OneLake diagnostics](https://learn.microsoft.com/en-us/fabric/onelake/onelake-diagnostics-overview) · [OneLake tenant settings](https://learn.microsoft.com/en-us/fabric/admin/service-admin-portal-onelake)

### Use Microsoft Defender for Cloud Apps

- Create session policies to control real-time access
- Block downloads of highly-confidential labeled content
- Monitor and alert on suspicious sharing patterns

## 5. Multi-Geo & Data Residency

### Plan Capacity Placement for Sovereignty

- Place capacities in Azure regions that satisfy data residency requirements
- Understand that metadata and some processing may reside in the home tenant region
- Document which workspaces are assigned to which geo-located capacities
- Use multi-geo only when required — it adds operational complexity

## 6. Operational Security

### Secure Admin Access

- Limit Fabric Admin role to minimal number of users (< 5 recommended)
- Require PIM (Privileged Identity Management) for admin role activation
- Review admin actions in audit logs weekly

### Protect Development Environments

- Use separate workspaces for dev/test/prod with different security postures
- Don't copy production data (especially PII) to dev workspaces without anonymization
- Use deployment pipelines with approval gates between environments

### Keep Gateways Patched

- Auto-update on-premises data gateways when possible
- Monitor gateway health and connectivity
- Use gateway clusters for high availability
- Restrict gateway admin access to infrastructure team only
