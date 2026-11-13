# FabricSecurity runbooks

[Solutions catalog](../README.md)

| Detail | Value |
|--------|-------|
| Type | Guide |
| Primary category | Fabric Administration, Governance, and Security |
| Related workloads | OneLake, Data Warehouse, Data Engineering, Real-Time Intelligence, Power BI, Fabric IQ |
| Public starting point | [nikosoukov/FabricSecurity](https://github.com/nikosoukov/FabricSecurity) |

## Scenario

A platform team needs workload-specific security procedures rather than a single platform checklist.

## Recommended approach

Use the upstream index to choose a workload series, starting with the OneLake permission model. The community runbooks cover identity, networking, data access, protection, and governance, with prerequisites, validation, limitations, and rollback sections.

Apply one control at a time in a test workspace, alongside this repository's security guidance. Keep the runbooks and diagrams upstream.

## Prerequisites and watch-outs

- Community recommendations are not official Microsoft requirements or proof that a configuration is supported.
- Some upstream entries flag differences from public documentation. Treat those differences as unresolved; follow current Microsoft Learn rather than adopting undocumented behavior as confirmed support.
- Validate the relevant workload, identity, network path, permissions, and preview limitations before changing controls.
- Review reuse terms before copying material. A linked checklist is not a security certification.

## Validation before use

- Test both intended access and denied access using representative identities.
- Verify the actual data-access paths, not just portal visibility.
- Record results and rehearse rollback without weakening unrelated controls.

## Related guidance

- [Security best practices](../../security/best-practices.md)
- [Network security patterns](../../security/network-security.md)
- [Governance by layer](../../best-practices/governance.md)

## Microsoft Learn

- [Security in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/security/security-overview)
