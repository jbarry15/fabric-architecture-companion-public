# Private-network deployment

[Solutions catalog](../../README.md)

| Detail | Value |
|--------|-------|
| Type | Reference implementation |
| Primary category | CI/CD |
| Related workloads | Data Engineering, platform networking and identity |
| Public starting point | [Fabric CI/CD with build agents inside your network](https://github.com/yeshwantmuralidhar/Microsoft-Fabric-cicd-private-build-agents) |

## Scenario

A Fabric workspace blocks public inbound access, but the team still needs reviewed, repeatable notebook deployments from source control.

## Recommended approach

Evaluate the public community example for a self-hosted Azure DevOps agent inside an approved network, using workspace-level Private Link, private DNS, and the VM's managed identity.

1. Review the upstream runbook with Fabric, Entra, network, and Azure DevOps administrators.
2. Establish the private endpoint and DNS path. Confirm that the agent can resolve and reach the intended workspace endpoint.
3. Grant the deployment identity only the required tenant and workspace access, then complete read-only preflight checks.
4. Review the notebook definition and explicitly enable a test deployment.
5. Confirm the deployed definition and keep the previous reviewed revision available for recovery.

## Prerequisites and watch-outs

- The example requires Azure Repos, a dedicated self-hosted Windows agent, private networking, and managed-identity permissions. Its test workspace must use supported F capacity and have no Power BI semantic models or deployment-pipeline assignment.
- **The implementation targets one standalone notebook**, without Lakehouse, Environment, or connection dependencies. It is not a complete multi-item release system.
- Microsoft Learn states that Fabric deployment pipelines are not supported for workspaces with inbound access protection. This example publishes definitions through APIs instead.
- A private agent-to-Fabric path does not eliminate Azure DevOps polling, source checkout, authentication, or update egress requirements.
- Confirm support for the exact APIs, identity, item types, and endpoint routing. Do not assume a generic public API hostname uses the workspace private endpoint.
- This is a community reference, not a Microsoft support commitment. Review upstream reuse terms before copying scripts.

## Validation before use

- Verify DNS resolution, connectivity, identity, and read-only access from the actual agent.
- Deploy only to the intended test workspace and compare the resulting definition with the reviewed source.
- Test the recovery procedure and reject unintended targets before considering wider rollout.

## Related guidance

- [CI/CD deployment best practices](../../../best-practices/ci-cd-deployment.md)
- [Network security patterns](../../../security/network-security.md)
- [Dev, Test, and Prod deployment](dev-test-prod-deployment.md)

## Microsoft Learn

- [Network security for CI/CD](https://learn.microsoft.com/en-us/fabric/cicd/cicd-security)
- [Workspace-level Private Link supported scenarios](https://learn.microsoft.com/en-us/fabric/security/security-workspace-level-private-links-support)
- [Fabric REST API identity support](https://learn.microsoft.com/en-us/rest/api/fabric/articles/identity-support)
