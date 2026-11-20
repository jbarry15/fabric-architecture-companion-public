# Microsoft Rayfin

[Fabric Apps solutions](../README.md)

| Detail | Value |
|--------|-------|
| Type | Guide |
| Primary workload | Fabric Apps |
| Related workloads | Databases in Fabric, Power BI |
| Public starting point | [microsoft/rayfin](https://github.com/microsoft/rayfin) |

## Scenario

A developer needs a public starting point for the Rayfin SDK, CLI, project templates, and agent-assisted development workflow before building a Fabric app.

## Recommended approach

Use the Microsoft repository to find current packages, templates, documentation, and contribution guidance. Keep the upstream project canonical rather than copying its SDK or examples into this companion.

1. Follow the public Microsoft Learn setup tutorial in a dedicated development workspace.
2. Choose a template, review its generated models and authorization policies, and record the CLI and package versions.
3. Keep application changes in source control. Review generated code and deployment targets before publishing or sharing the app.

## Prerequisites and watch-outs

- **Fabric Apps and Rayfin are generally available.** Confirm tenant enablement, workspace capacity, supported region, and creation/deployment permissions before deploying to Fabric.
- By default, `npx rayfin dev` can provision or reuse a Fabric backend. [Rayfin Local](https://github.com/microsoft/rayfin-local) runs the app and backend locally without provisioning a Fabric workspace. Connectors that access Fabric still require connectivity and credentials.
- Template layouts and CLI options change. Follow the selected template and current documentation rather than mixing snippets from different versions.
- Fabric sign-in does not replace application authorization. Review data policies, item permissions, and frontend assets; never embed secrets in browser-delivered code.

## Validation before use

- Confirm the intended workspace, deployed services, and application sign-in behavior.
- Test authorized and unauthorized access with representative identities.
- Record versions, review expected costs, and plan cleanup before expanding the prototype.

## Related guidance

- [Fabric Apps best practices](../../../best-practices/fabric-apps.md)
- [Rayfin onboarding lab](rayfin-onboarding-2026.md)

## Microsoft Learn

- [Fabric Apps overview and prerequisites](https://learn.microsoft.com/en-us/fabric/apps/overview)
- [Create and deploy a Fabric app with the CLI](https://learn.microsoft.com/en-us/fabric/apps/create-app-with-cli)
- [Rayfin SDK overview](https://learn.microsoft.com/en-us/javascript/api/fabric-apps-sdk-javascript/rayfin-overview)
