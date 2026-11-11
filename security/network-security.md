# Fabric Network Security Patterns

Guidance for securing inbound and outbound connectivity in Microsoft Fabric deployments.

## Inbound Connectivity (Users → Fabric)

### Pattern 1: Public Access with Conditional Access (Default)

```
Users ──── Internet ──── Microsoft Entra ID ──── Fabric Service
                              │
                         Conditional Access
                         ├─ MFA required
                         ├─ Compliant device
                         ├─ Named locations
                         └─ Session controls
```

**When to use**: Most organizations, remote workforce, dev/test environments.

**Controls available**:
- MFA enforcement
- Device compliance (Intune-managed)
- Named location (IP-based) restrictions
- Sign-in risk policies
- Session duration limits

> **Reference**: [Conditional Access in Fabric](https://learn.microsoft.com/en-us/fabric/security/security-conditional-access)

---

### Pattern 2: Private Link (Restricted Inbound Access)

```
Corporate VNet                         Fabric Service
┌──────────────────┐                  ┌──────────────┐
│  Users on VNet   │─── Private ─────▶│ Private      │
│  or VPN/ER       │    Endpoint      │ Endpoint     │
└──────────────────┘                  └──────────────┘

Public Internet ────── BLOCKED ──────▶ Fabric Tenant
```

**When to use**: Regulated industries, government, highly sensitive data, or workspaces that must be reachable only through private endpoints.

**Key considerations**:
- **Tenant-level Private Link** affects all workspaces in the tenant
- **Workspace-level Private Link** restricts selected workspaces without forcing every workspace in the tenant behind private endpoints
- All users of restricted tenant/workspace scopes must access via private network (VPN/ExpressRoute/private endpoint path)
- Embedded reports, external sharing, public APIs, DNS, and unsupported Fabric scenarios can be impacted
- Private Link protects inbound access to Fabric; it doesn't secure Fabric's outbound path to external data sources
- Test thoroughly before enabling public internet blocking

> **References**: [Private Links in Fabric](https://learn.microsoft.com/en-us/fabric/security/security-private-links-overview) · [Workspace-level Private Links](https://learn.microsoft.com/en-us/fabric/security/security-workspace-level-private-links-overview) · [Protect inbound traffic](https://learn.microsoft.com/en-us/fabric/security/protect-inbound-traffic)

#### Item types excluded from inbound restricted workspaces

Two item types can't exist in a workspace configured to deny inbound public access, and both change the target architecture:

- **Semantic models** — a workspace containing a Power BI semantic model can't have workspace-level private links enabled, and a restricted workspace can't have one created in it
- **Deployment pipelines** — a workspace assigned to a deployment pipeline can't be restricted

If a workspace contains any unsupported item type, inbound public access can't be restricted at all.

#### Serving BI from inbound restricted data

**Direct Lake is not supported against data sources in inbound restricted workspaces.** This is the single most consequential limitation for a Medallion architecture, because it conflicts with the default recommendation to serve Gold through Direct Lake.

Two viable designs:

| Design | How it works | Trade-off |
|--------|--------------|-----------|
| **Scope restriction to sensitive layers** (preferred where the control objective allows) | Apply workspace-level private links to Bronze/Silver, keep Gold and the BI workspaces unrestricted | Direct Lake, star schema serving, and the standard lifecycle all behave normally |
| **Restrict Gold too** | Semantic models live in an open workspace and reach the restricted lakehouse or warehouse through an **on-premises data gateway** in **Import or DirectQuery** mode, using the workspace FQDN SQL analytics endpoint | Loses Direct Lake; adds a gateway cluster to size, operate, and make highly available; Import caches restricted data into a model in an open workspace |

Implementation notes for the gateway design:
- Connect to the SQL analytics endpoint FQDN, which inserts `z{xy}` (the first two characters of the workspace ID) into the standard warehouse connection string
- Don't author through the OneLake catalog in Power BI Desktop or web modeling — it creates a Direct Lake model that can't connect. Use the SQL Server/Azure SQL connectors instead
- Power BI Desktop must run on a machine with private network access to the target workspace

> **Reference**: [Access inbound restricted lakehouse data from Power BI using an on-premises data gateway](https://learn.microsoft.com/en-us/fabric/security/security-workspace-private-links-example-on-premises-data-gateway)

#### Choosing tenant-level vs workspace-level

Neither scope is free, and tenant-level private links carry costs well beyond networking. Enabling the **Azure Private Link** tenant setting disables Publish to Web, PDF/PowerPoint export, and Copilot, returns only partial usage metrics, and disables Spark starter pools (jobs run on on-demand custom pools in a workspace managed VNet, so expect slower starts). Additionally enabling **Block Public Internet Access** disables email subscriptions and Warehouse visual query, and fails usage metrics refresh. Cross-tenant shortcuts and OneLake data sharing aren't supported over Private Link at either scope.

| Requirement | Scope to use |
|-------------|--------------|
| Only specific workspaces hold sensitive data | Workspace-level, applied to those workspaces only |
| Regulatory mandate covering the entire tenant | Tenant-level, accepting the feature trade-offs above |
| Semantic models and Direct Lake must keep working | Leave those workspaces unrestricted; use Conditional Access and OneLake security as the control |

Treat the per-workspace cost as real: one private link service per workspace (100 private endpoints per workspace, 500 PLS per tenant, 10 created per minute), plus a private endpoint and DNS record each. Forecast this against the planned workspace count, including ephemeral feature workspaces, before standardizing on workspace-level private links.

---

## Outbound Connectivity (Fabric → Data Sources)

### Pattern A: Trusted Workspace Access (ADLS Gen2)

```
Fabric Workspace              ADLS Gen2 (Firewall Enabled)
┌─────────────────┐          ┌─────────────────────────────┐
│ Workspace       │─── Trusted ─▶│ Storage Account          │
│ Identity (MSI)  │   Access     │ ├─ Public: Selected VNets│
└─────────────────┘              │ └─ Resource Instance Rule │
                                 └─────────────────────────────┘
```

**Requirements**: F SKU capacity, workspace identity configured.

**When to use**: ADLS Gen2 sources that need firewall protection without gateway overhead.

> **Reference**: [Trusted Workspace Access](https://learn.microsoft.com/en-us/fabric/security/security-trusted-workspace-access)

---

### Pattern B: Managed Private Endpoints (Supported Fabric Workloads)

```
Fabric Managed VNet                    Azure PaaS Service
┌─────────────────────┐               ┌──────────────────┐
│ Spark Cluster       │── Managed ────▶│ Azure SQL DB     │
│ (Data Engineering / │   Private      │ (Public access   │
│  Data Science)      │   Endpoint     │  disabled)       │
│ Eventstream         │                │ Event Hubs / IoT │
└─────────────────────┘               └──────────────────┘
```

**When to use**: Spark notebooks/jobs accessing supported private Azure sources, or Eventstream securely streaming from Azure Event Hubs or Azure IoT Hub with public access disabled.

**Key considerations**:
- Spark support and Eventstream support have different source matrices; Eventstream managed private endpoints are GA for Azure Event Hubs and Azure IoT Hub only
- Must approve the private endpoint on the target resource
- Managed VNet must be enabled on the workspace
- OneLake shortcuts to ADLS Gen2 and Azure Blob Storage don't yet support managed private endpoints
- Regional and Private Link settings can affect Spark startup, starter pools, and workspace migration

> **References**: [Managed Private Endpoints](https://learn.microsoft.com/en-us/fabric/security/security-managed-private-endpoints-overview) · [Eventstream managed private endpoints](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/set-up-private-endpoint)

---

### Pattern C: On-Premises Data Gateway

```
On-Premises Network                 Microsoft Cloud
┌────────────────────┐             ┌─────────────────┐
│ SQL Server         │◀── Gateway ─│ Fabric Service  │
│ File Shares        │   (outbound │                 │
│ Oracle, SAP, etc.  │    HTTPS)   │                 │
└────────────────────┘             └─────────────────┘
       │
  Data Gateway
  (Windows Service)
```

**When to use**: On-premises data sources, legacy systems, non-Azure environments.

**Best practices**:
- Install on a dedicated VM (not a user workstation)
- Use gateway clusters for HA (2+ nodes)
- Keep auto-update enabled
- Monitor health via the Power Platform Admin Center

---

### Pattern D: VNet Data Gateway

```
Azure VNet                          Microsoft Cloud
┌────────────────────┐             ┌─────────────────┐
│ Azure SQL MI       │◀── VNet ────│ Fabric Service  │
│ Azure VM (SQL)     │   Gateway   │                 │
│ Private Endpoints  │             │                 │
└────────────────────┘             └─────────────────┘
```

**When to use**: Azure-resident sources behind VNets where managed private endpoints aren't available or don't apply to the workload/connector, such as selected dataflows, pipelines, and Power BI refresh scenarios.

> For gateway sizing, HA/clustering, updates, and keeping data in-region with **bring your own relay**, see [Data Gateways Best Practices](../best-practices/data-gateways.md).

---

### Pattern E: Service Tags

```
Azure VNet with NSG                 Microsoft Cloud
┌────────────────────┐             ┌─────────────────┐
│ Azure SQL VM       │◀── Service ─│ Fabric Service  │
│ REST API hosts     │    Tag:     │ (DataFactory,   │
│                    │   "Power    │  PowerBI)       │
│ NSG Rule: Allow    │   Platform" │                 │
│ outbound to Fabric │             │                 │
└────────────────────┘             └─────────────────┘
```

**When to use**: Sources in Azure VNets that need to allow Fabric traffic without gateways.

> **Reference**: [Service Tags](https://learn.microsoft.com/en-us/fabric/security/security-service-tags)

---

## Decision Matrix: Which Outbound Pattern?

| Source Location | Source Type | Recommended Pattern |
|-----------------|------------|---------------------|
| Azure (PaaS, firewalled) | ADLS Gen2 | Trusted Workspace Access |
| Azure (PaaS, firewalled) | SQL, Cosmos, ADLS, etc. for Spark | Managed Private Endpoints |
| Azure (PaaS, firewalled) | Event Hubs or IoT Hub for Eventstream | Managed Private Endpoints |
| Azure (PaaS, firewalled) | SQL, etc. (Pipelines/Dataflows/Power BI) | VNet Data Gateway or on-premises data gateway, depending on connector and network path |
| Azure (VNet, IaaS) | SQL VM, REST APIs | Service Tags or VNet Gateway |
| On-premises | Any | On-Premises Data Gateway |
| SaaS / Internet | REST APIs | Direct (no gateway needed) |

## References

- [Fabric Security Overview](https://learn.microsoft.com/en-us/fabric/security/security-overview)
- [Protect Inbound Traffic](https://learn.microsoft.com/en-us/fabric/security/protect-inbound-traffic)
- [Workspace-Level Private Links](https://learn.microsoft.com/en-us/fabric/security/security-workspace-level-private-links-overview)
- [Supported scenarios and limitations for workspace-level private links](https://learn.microsoft.com/en-us/fabric/security/security-workspace-level-private-links-support)
- [Access inbound restricted lakehouse data from Power BI using an OPDG](https://learn.microsoft.com/en-us/fabric/security/security-workspace-private-links-example-on-premises-data-gateway)
- [Network security for CI/CD](https://learn.microsoft.com/en-us/fabric/cicd/cicd-security)
- [Managed VNets Overview](https://learn.microsoft.com/en-us/fabric/security/security-managed-vnets-fabric-overview)
- [Managed Private Endpoints](https://learn.microsoft.com/en-us/fabric/security/security-managed-private-endpoints-overview)
- [Eventstream managed private endpoints](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/set-up-private-endpoint)
- [Trusted Workspace Access](https://learn.microsoft.com/en-us/fabric/security/security-trusted-workspace-access)
- [OneLake Security](https://learn.microsoft.com/en-us/fabric/onelake/security/get-started-security)
