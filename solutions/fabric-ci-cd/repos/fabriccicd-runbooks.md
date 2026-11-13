# FabricCICD runbook series

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Guide |
| Primary category | Fabric CI/CD |
| Related workloads | Cross-workload development, deployment, and platform operations |
| Public starting point | [nikosoukov/FabricCICD: fabric-cicd](https://github.com/nikosoukov/FabricCICD/tree/main/fabric-cicd) |

## Scenario

A team needs a structured learning and implementation reference for Fabric source control, development isolation, environment promotion, and release automation.

## Recommended approach

Start with the [series overview](https://github.com/nikosoukov/FabricCICD/blob/main/fabric-cicd/Fabric-CICD-00-Series-Overview.md), then select the runbooks relevant to the team's release process.

The community series contains 25 runbooks across five areas: Git foundations, development management, environment promotion, release automation, and secured environments. Its entries include prerequisites, steps, validation, limitations, and rollback guidance.

Use the series alongside this repository's CI/CD guidance. Keep the upstream runbooks and diagrams canonical rather than copying them here.

## Prerequisites and watch-outs

- This is a community-authored guide series, not an official Microsoft support statement or the `fabric-cicd` Python package itself.
- Confirm current Microsoft Learn requirements for the selected item types, APIs, identities, Git provider, and network configuration. Do not treat the series' support tables or dated claims as tenant-independent guarantees.
- Review the applicable tenant settings, workspace and repository permissions, environment bindings, and release approvals before applying a runbook.
- Review upstream reuse terms before copying material. A linked runbook is not evidence that its workflow has been deployed or validated in your environment.

## Validation before use

- Exercise the chosen workflow in an isolated development or test workspace.
- Verify target bindings and permissions as well as deployment success.
- Rehearse the documented recovery path, including any data recovery beyond restoring item definitions.

## Related guidance

- [CI/CD deployment best practices](../../../best-practices/ci-cd-deployment.md)
- [fabric-cicd Python deployment library](fabric-cicd.md) - the publishing tool, distinct from this community runbook series
- [Dev, Test, and Prod deployment](dev-test-prod-deployment.md)
- [Private-network deployment](private-network-deployment.md)

## Microsoft Learn

- [Lifecycle management best practices](https://learn.microsoft.com/en-us/fabric/cicd/best-practices-cicd)
