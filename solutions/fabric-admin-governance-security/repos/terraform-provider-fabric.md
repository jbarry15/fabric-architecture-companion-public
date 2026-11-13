# Terraform provider for Microsoft Fabric

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Guide |
| Primary category | Fabric Administration, Governance, and Security |
| Related workloads | Fabric CI/CD, workspace provisioning, identity and access |
| Public starting point | [microsoft/terraform-provider-fabric](https://github.com/microsoft/terraform-provider-fabric) |

> **Support boundary:** The upstream README explicitly states that this open-source provider is not covered by Microsoft Fabric or Microsoft support policies. Use the project's issue process; do not infer a production support commitment from its publisher.

## Scenario

A platform team wants reviewable infrastructure-as-code plans for supported Fabric resources rather than relying on repeated portal configuration.

## Recommended approach

Start with the upstream [provider documentation](https://registry.terraform.io/providers/microsoft/fabric/latest/docs), pin a reviewed version, and test a small provisioning plan in an isolated environment.

Separate platform provisioning from frequent content releases. Where appropriate, use this provider for infrastructure and the [fabric-cicd library](../../fabric-ci-cd/repos/fabric-cicd.md) for item-definition deployment, with clear ownership of each resource.

## Prerequisites and watch-outs

- Confirm support for each resource, operation, and authentication method in the selected provider version and underlying Fabric APIs.
- Tenant approval, API access, and resource permissions remain necessary; installing the provider does not grant them.
- Review plans for replacement or deletion, protect state and plan files, and keep credentials out of configuration and Git.
- Review telemetry settings, resource costs, and the project's support boundary before adopting an example or bootstrap script.

## Validation before use

- Inspect the plan before applying and confirm the intended target and resource scope.
- Re-run planning to detect unexpected drift after provisioning.
- Test change and recovery procedures without destroying shared resources.

## Related guidance

- [Governance by layer](../../../best-practices/governance.md)
- [CI/CD deployment best practices](../../../best-practices/ci-cd-deployment.md)
- [Security best practices](../../../security/best-practices.md)

## Microsoft Learn

- [End-to-end automation with Terraform and fabric-cicd](https://learn.microsoft.com/en-us/fabric/cicd/tutorial-end-to-end-automation)
- [Fabric REST API identity support](https://learn.microsoft.com/en-us/rest/api/fabric/articles/identity-support)
