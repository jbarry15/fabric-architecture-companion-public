# fabric-cicd Python deployment library

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Guide |
| Primary category | Fabric CI/CD |
| Related workloads | Cross-workload deployment and platform automation |
| Public starting point | [microsoft/fabric-cicd](https://github.com/microsoft/fabric-cicd) |

## Scenario

A release team wants a Python publishing library for source-controlled Fabric item definitions instead of implementing every deployment API call itself.

## Recommended approach

Use the upstream [library documentation](https://microsoft.github.io/fabric-cicd/) to select supported item types, configure environment-specific values, and exercise deployment in an isolated target workspace.

This is the deployment library, **not** the similarly named [FabricCICD runbook series](fabriccicd-runbooks.md). The runbooks explain workflows; the [Dev, Test, and Prod reference](dev-test-prod-deployment.md) demonstrates a release pattern that uses the library. Those references are complementary, not alternate copies of this project.

## Prerequisites and watch-outs

- Pin a reviewed library version and verify its supported items, API identities, tenant settings, and permissions. Its support matrix is not identical to native Git integration or deployment pipelines.
- Review target bindings, connection configuration, publishing scope, and cleanup operations before a release. Unpublishing removed items is a potentially destructive action.
- Treat deployment of item definitions separately from data migration, credentials, access policies, and runtime validation.
- The package does not replace release approvals, recovery planning, or the upstream project's support policy.

## Validation before use

- Deploy a small dependency chain and confirm that it uses only the intended test resources.
- Compare the resulting definitions and bindings with reviewed source.
- Rehearse failed deployments and cleanup without affecting unrelated items.

## Related guidance

- [CI/CD deployment best practices](../../../best-practices/ci-cd-deployment.md)
- [Terraform provider for Fabric](../../fabric-admin-governance-security/repos/terraform-provider-fabric.md)
- [Private-network deployment reference](private-network-deployment.md)

## Microsoft Learn

- [End-to-end automation with Terraform and fabric-cicd](https://learn.microsoft.com/en-us/fabric/cicd/tutorial-end-to-end-automation)
- [Lifecycle management best practices](https://learn.microsoft.com/en-us/fabric/cicd/best-practices-cicd)
