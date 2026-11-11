# Anti-Pattern: Direct-to-Production Changes

## Summary

Making changes directly in the production workspace without going through Git integration or deployment pipelines.

## Why it's a problem

| Impact | Severity |
|--------|----------|
| No audit trail of who changed what and when | 🔴 High |
| No rollback capability if something breaks | 🔴 High |
| No testing — changes hit live users immediately | 🔴 High |
| Bypasses review process — errors go unnoticed | 🔴 High |

## What this looks like

- Editing a notebook directly in the production workspace
- Modifying a semantic model in prod "just to fix a quick bug"
- Creating new pipelines directly in the production workspace
- Changing deployment rules or data sources without any tracked change

## What to do instead

1. **Connect all workspaces to Git** — Every workspace should have a Git repo backing it
2. **Use deployment pipelines** — Dev → Test → Prod with approval gates
3. **Lock production workspace access** — Only deployment service principals should write to prod
4. **Treat prod as read-only for humans** — Developers work in dev workspaces; pipelines promote changes

## Decision framework

| Scenario | Action |
|----------|--------|
| Urgent production fix needed | Create hotfix branch → test in dev → fast-track through pipeline |
| "It's just a small change" | Still goes through the pipeline — small changes can still break things |
| Exploratory analysis | Use a separate workspace; never explore in prod |

## References

- [CI/CD best practices](../best-practices/ci-cd-deployment.md)
- [Deployment pipelines overview](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/intro-to-deployment-pipelines)
- [Git integration](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration)
