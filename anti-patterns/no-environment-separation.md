# Anti-Pattern: No Environment Separation (Single Workspace for Dev and Prod)

## Summary

Using a single workspace for both development and production, or sharing the same database/data sources across all lifecycle stages.

## Why it's a problem

| Impact | Severity |
|--------|----------|
| Dev queries impact production performance | 🔴 High |
| Can't test independently — changes affect live users | 🔴 High |
| Can't use deployment pipelines (requires separate workspaces) | 🔴 High |
| No isolation for experimentation or breaking changes | 🟡 Medium |
| Impossible to apply different security policies per stage | 🟡 Medium |

## What this looks like

- One workspace called "Analytics" where reports, dev notebooks, and production pipelines coexist
- Dev and prod notebooks reading from the same Lakehouse
- Test pipelines running against production data sources
- All team members having the same access level regardless of stage

## What to do instead

### Workspace separation

| Stage | Workspace | Purpose | Access |
|-------|-----------|---------|--------|
| **Dev** | `ws-analytics-dev` | Development, experimentation | Engineers |
| **Test** | `ws-analytics-test` | Validation with production-like data | Engineers + QA |
| **Prod** | `ws-analytics-prod` | Serving business users | Limited deploy access; broad read access |

### Database separation

| Stage | Data source | Notes |
|-------|-------------|-------|
| Dev | Subset/anonymized copy | Safe for experimentation |
| Test | Production-like volume | Validate performance at scale |
| Prod | Live data | Protected, governed |

## Decision framework

| Scenario | Recommendation |
|----------|---------------|
| Small team, just getting started | Minimum 2 workspaces (dev + prod) |
| Enterprise deployment | 3+ workspaces per project (dev/test/prod) |
| Multiple teams on same data | Separate workspaces per team per stage |

## References

- [CI/CD best practices](../best-practices/ci-cd-deployment.md)
- [Deployment patterns](../reference-architectures/deployment-patterns.md)
- [Fabric deployment pipelines](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/intro-to-deployment-pipelines)
