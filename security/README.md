# Microsoft Fabric Security

Security guidance, best practices, and anti-patterns for Microsoft Fabric deployments.

## Official References

| Resource | Description |
|----------|-------------|
| [Fabric Security White Paper (PDF)](https://aka.ms/fabricsecuritywhitepaper) | End-to-end security overview — covers default SaaS protections, data-at-rest/in-transit, and governance |
| [Security White Paper Landing Page](https://learn.microsoft.com/en-us/fabric/security/white-paper-landing-page) | Online companion to the downloadable white paper |
| [Security in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/security/security-overview) | Always-current online security documentation |
| [Security Fundamentals](https://learn.microsoft.com/en-us/fabric/security/security-fundamentals) | Deep dive into authentication, encryption, and platform isolation |
| [Permission Model](https://learn.microsoft.com/en-us/fabric/security/permission-model) | Workspace roles, item permissions, and sharing |
| [OneLake Data Security](https://learn.microsoft.com/en-us/fabric/onelake/security/get-started-security) | Data-plane security — define table/folder, row-, and column-level access once and enforce it across supported engines |
| [OneLake Security Access Control Model](https://learn.microsoft.com/en-us/fabric/onelake/security/data-access-control-model) | Current OneLake role model, supported items, default roles, and engine support |
| [Workspace-Level Private Links](https://learn.microsoft.com/en-us/fabric/security/security-workspace-level-private-links-overview) | Restrict inbound access for selected workspaces without applying tenant-wide Private Link |
| [Trusted Workspace Access](https://learn.microsoft.com/en-us/fabric/security/security-trusted-workspace-access) | Secure outbound access from specific Fabric workspaces to firewalled ADLS Gen2 accounts |

## Contents

| Document | Description |
|----------|-------------|
| [Best Practices](best-practices.md) | Security best practices for Fabric deployments |
| [Anti-Patterns](anti-patterns.md) | Common security mistakes and how to avoid them |
| [Network Security](network-security.md) | Inbound/outbound connectivity patterns and controls |

## Security Pillars in Fabric

```
┌────────────────────────────────────────────────────────────────┐
│                    Microsoft Fabric Security                    │
├────────────┬────────────┬────────────┬────────────┬───────────┤
│  Identity  │  Network   │    Data    │ Governance │ Monitoring│
│  & Access  │  Security  │ Protection │  & Compliance│& Audit  │
├────────────┼────────────┼────────────┼────────────┼───────────┤
│ Entra ID   │ Private    │ Encryption │ Purview    │ Audit     │
│ Cond.Access│ Links      │ at Rest    │ Labels     │ Logs      │
│ Workspace  │ Managed    │ RLS/CLS/OLS│ DLP        │ Admin     │
│ Roles      │ VNets      │ Customer   │ Lineage    │ Portal    │
│ Item Perms │ Service    │ Managed    │ Retention  │ Defender  │
│ Sharing    │ Tags       │ Keys       │ Policies   │ for Cloud │
└────────────┴────────────┴────────────┴────────────┴───────────┘
```

## Quick Decision: Which Security Controls Do I Need?

| Scenario | Minimum Controls |
|----------|-----------------|
| Dev/Test with non-sensitive data | Workspace roles, Entra auth |
| Internal analytics with PII | + OneLake security, semantic-model RLS/OLS, sensitivity labels, Conditional Access |
| Regulated industry (finance, health) | + tenant- or workspace-level Private Link, Managed VNets, managed private endpoints, CMK, DLP, audit logging |
| Multi-tenant ISV or cross-tenant sharing | + item-level sharing, OneLake data sharing/shortcuts, OLS/RLS, multi-geo, tenant isolation |
