# Dev, Test, and Prod deployment

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Reference implementation |
| Primary category | CI/CD |
| Related workloads | Data Factory, Data Engineering, Power BI |
| Public starting point | [Microsoft Fabric SDLC Patterns](https://github.com/michaeldeongreen/microsoft-fabric-sdlc-patterns) |

## Scenario

A team needs isolated development and repeatable releases across Dev, Test, and Prod without manually copying artifacts or editing production.

## Recommended approach

Evaluate the public community reference for Git-based development and GitHub Actions deployments using `fabric-cicd`. Treat its branching and release design as an example to adapt, not a universal Fabric requirement.

1. Define environment workspaces, deployment identities, repository protections, approvals, and operational owners.
2. Follow the upstream setup and development process, including its feature-workspace and environment-reference conventions.
3. Prove the default deployment path in Test before evaluating other deployment methods.
4. Promote reviewed changes with explicit environment configuration. Validate bindings, connections, data-loading jobs, and semantic-model behavior in the target.
5. Gate production on acceptance evidence and a rehearsed recovery plan.

## Prerequisites and watch-outs

- Arrange repository administration, GitHub Actions and environment access, Fabric capacity/workspaces, and Entra/Fabric identity and tenant approvals before setup.
- Confirm support for every item type and API used by the selected publisher. Git integration, deployment pipelines, and `fabric-cicd` do not have identical support matrices.
- Item definitions are not a full backup of data, credentials, permissions, or dependent resources. Include those concerns in the release plan.
- Environment references must resolve to the intended targets; a successful publish alone does not establish a successful release.
- The upstream documentation distinguishes implemented examples from architectural extensions and production controls. Do not assume every discussed feature is included or live-validated.
- This is a community reference, not official Microsoft architecture guidance. Review reuse terms and pin a reviewed tool version before adopting its scripts.

## Validation before use

- Release a small representative dependency chain to Test and verify source control, deployment, and runtime outcomes.
- Confirm that Test does not accidentally use production connections or data.
- Exercise failed-validation gates and recovery, then retain approval and deployment evidence for production.

## Related guidance

- [CI/CD deployment best practices](../../../best-practices/ci-cd-deployment.md)
- [Deployment patterns](../../../reference-architectures/deployment-patterns.md)
- [Private-network deployment](private-network-deployment.md) - a narrower option for inbound-restricted workspaces

## Microsoft Learn

- [Plan CI/CD for Fabric solutions](https://learn.microsoft.com/en-us/fabric/fundamentals/understand-best-practices-fabric-cicd)
- [Lifecycle management best practices](https://learn.microsoft.com/en-us/fabric/cicd/best-practices-cicd)
