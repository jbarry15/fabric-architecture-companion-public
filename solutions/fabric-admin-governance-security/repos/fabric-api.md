# FabricAPI inventory and audit examples

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Reference implementation |
| Primary category | Fabric Administration, Governance, and Security |
| Related workloads | Power BI, Microsoft Purview |
| Public starting point | [Hapstanny/FabricAPI](https://github.com/Hapstanny/FabricAPI) |

## Scenario

An administrator wants Python and CLI examples for workspace inventory, Power BI activity, or Microsoft Purview audit extraction.

## Recommended approach

Start with the upstream read-only discovery examples and choose the API family needed for the task. The project separates Fabric REST, Power BI REST, and Microsoft Graph audit workflows rather than treating them as one endpoint or token audience.

Review the selected command, authentication path, and export destination before running it. Keep example code and installation instructions upstream.

## Prerequisites and watch-outs

- Check the exact API's identity support, permissions, tenant settings, consent, and resource access. Successful sign-in alone does not authorize an operation.
- Audit extraction has different permissions from ordinary workspace inventory. Review administrator setup scripts separately; do not grant broad audit access just to list items.
- Inventory, activity events, and audit records answer different questions. Observed counts do not establish complete coverage or business impact.
- Audit exports can contain sensitive identities and activity details. Protect them, define retention, and keep exports and credentials out of Git.

## Validation before use

- Reconcile a small inventory or bounded audit window with the source service.
- Verify pagination, authorization failures, and output scope.
- Confirm that only approved identities can read the resulting exports.

## Related guidance

- [Governance by layer](../../../best-practices/governance.md)
- [Security best practices](../../../security/best-practices.md)

## Microsoft Learn

- [Fabric REST API identity support](https://learn.microsoft.com/en-us/rest/api/fabric/articles/identity-support)
- [Create a Microsoft Graph audit log query](https://learn.microsoft.com/en-us/graph/api/security-auditcoreroot-post-auditlogqueries?view=graph-rest-1.0)
