# Fabric Jumpstart

[Other Fabric solutions](../README.md)

| Detail | Value |
|--------|-------|
| Type | Reference implementation |
| Primary category | Other Fabric Solutions |
| Related workloads | Data Engineering, Data Factory, Power BI, cross-workload analytics |
| Public starting point | [microsoft/fabric-jumpstart](https://github.com/microsoft/fabric-jumpstart) |

## Scenario

A team wants a deployable example to explore a Fabric scenario without assembling every demonstration asset manually.

## Recommended approach

Choose a scoped example from the existing [Jumpstart catalog](https://jumpstart.fabric.microsoft.com/), then review its assets, prerequisites, and post-install steps in the upstream repository.

Install only into an explicitly selected evaluation workspace. The repository provides the Python installer and catalog website; these are entry points to the same project, not separate solutions to duplicate here.

## Prerequisites and watch-outs

- Review the chosen example's capacity, tenant, identity, connection, and external-service requirements. Availability and costs depend on the deployed components.
- Installation writes workspace items and can upload supplied data files. Inspect source and target configuration before running it.
- Review name-conflict options and require approval before overwriting existing items. Prefer an isolated workspace or distinct item prefix.
- Demonstration success is not production certification. Review sample-data reuse terms, ongoing costs, and resource cleanup before wider use.

## Validation before use

- Confirm that created items and uploaded files belong to the intended example and workspace.
- Complete manual setup and reconcile a representative result to its inputs.
- Record the resources created and test cleanup without removing unrelated work.

## Related guidance

- [End-to-end Medallion architecture](../../../reference-architectures/end-to-end-medallion.md)
- [Fabric Toolbox resources](../../../toolbox/README.md)
- [fabric-cicd deployment library](../../fabric-ci-cd/repos/fabric-cicd.md)

## Microsoft Learn

- [Lifecycle management best practices](https://learn.microsoft.com/en-us/fabric/cicd/best-practices-cicd)
