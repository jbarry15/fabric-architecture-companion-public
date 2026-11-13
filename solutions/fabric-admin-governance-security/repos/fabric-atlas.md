# Fabric Atlas workspace intelligence

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Reference implementation |
| Primary category | Fabric Administration, Governance, and Security |
| Related workloads | Fabric Apps, OneLake, Power BI |
| Public starting point | [fredgis/FabricAtlas](https://github.com/fredgis/FabricAtlas) |

## Scenario

A governance team needs a shared view of workspace inventory, lineage, recorded access, connections, and operational findings.

## Recommended approach

Evaluate the community application against a small, approved workspace scope. It collects metadata into validated snapshots and offers lineage, access-evidence, and change-review views.

Follow the upstream installation and coverage documentation. Reconcile findings with the Fabric portal and source APIs before using them to plan changes.

## Prerequisites and watch-outs

- The application uses Fabric Apps, which is preview. Confirm tenant enablement, capacity, deployment permissions, and the selected collector's API requirements.
- **Shared metadata is not automatically filtered to each viewer's source permissions.** Upstream documents that Atlas Scout users can see all synchronized workspaces and that the connection inventory is shared with Atlas users. Restrict the app audience and collection scope accordingly.
- Missing or partial metadata is not evidence that access is denied or a control is effective. Snapshot age and API coverage matter.
- Optional Scout and MCP integrations have separate setup and identity requirements. Review them before enabling access to governance metadata.

## Validation before use

- Compare a known dependency and recorded grant with their source evidence.
- Test what different app users can see before collecting sensitive metadata.
- Confirm collection freshness and clearly identify uncollected or unknown evidence.

## Related guidance

- [Governance by layer](../../../best-practices/governance.md)
- [Security best practices](../../../security/best-practices.md)

## Microsoft Learn

- [Lineage in Fabric](https://learn.microsoft.com/en-us/fabric/governance/lineage)
- [Fabric Apps overview and preview prerequisites](https://learn.microsoft.com/en-us/fabric/apps/overview)
