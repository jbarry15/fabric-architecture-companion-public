# Fabric Skills

Curated skills and agent enablement resources that help users operate over Microsoft Fabric from Copilot CLI, VS Code, Claude Code, Cursor, Windsurf, and other agentic developer surfaces.

## Overview

This section is for reusable Fabric skill resources that extend the architecture guidance in this repo into hands-on agent workflows. These resources should help teams inspect Fabric environments, automate repeatable tasks, analyze Fabric assets, or accelerate solution delivery using skill-guided workflows.

This section covers:

- [Skills for Fabric](https://github.com/microsoft/skills-for-fabric) - reusable AI assistant instructions for working with Microsoft Fabric workloads, APIs, query patterns, and operational best practices.
- [Power BI Agentic Development](https://github.com/data-goblin/power-bi-agentic-development) - community-maintained skills, subagents, and hooks for Power BI and Fabric development with Claude Code and GitHub Copilot CLI.

For live MCP server/tool access, use the root-level [Fabric MCP resources](../fabric-mcps/README.md) area.

## Skills for Fabric bundles

Use the `microsoft/skills-for-fabric` repo as the canonical source for the latest bundle contents, examples, release notes, and host-specific setup guidance. This repo lists the current installable bundles so users can quickly choose the right starting point.

| Bundle | Use it for |
|--------|------------|
| `fabric-skills` | Complete Microsoft Fabric skill bundle, including authoring, consumption, operations, migration, and end-to-end architecture skills. |
| `powerbi-authoring` | Authoring Power BI semantic models, reports, and PBIP workflows, including Power BI report planning, design, authoring, and management. |

> The former persona bundles `fabric-authoring`, `fabric-consumption`, and `fabric-operations` are retired. Existing installs can continue updating through their deprecated aliases, but new installs should use `fabric-skills`.

## Install with GitHub Copilot CLI

Add the public marketplace:

```bash
/plugin marketplace add microsoft/skills-for-fabric
```

Install the full Fabric bundle. Power BI report authoring is packaged separately.

```bash
/plugin install fabric-skills@fabric-collection
```

Or install the Power BI authoring bundle:

```bash
/plugin install powerbi-authoring@fabric-collection
```

## Update installed bundles

Update one installed bundle:

```bash
/plugin update fabric-skills@fabric-collection
```

Update all installed plugins from a terminal:

```bash
copilot plugin update --all
```

## Automatic update checking

GitHub Copilot CLI can auto-update the Fabric marketplace when `autoUpdate` is enabled in the user's personal settings file (`%USERPROFILE%\.copilot\settings.json` on Windows or `~/.copilot/settings.json` on macOS/Linux):

```json
{
  "extraKnownMarketplaces": {
    "fabric-collection": {
      "source": { "source": "github", "repo": "microsoft/skills-for-fabric" },
      "autoUpdate": true
    }
  }
}
```

Auto-update is handled by the agent host, not by a skill in the repo. It should be configured only in personal or managed settings, not as committed project configuration.

## When to use which bundle

| Need | Recommended bundle |
|------|--------------------|
| Broad Fabric architecture, migration, and implementation support | `fabric-skills` |
| Build or automate Fabric items | `fabric-skills` |
| Explore, query, or monitor existing Fabric assets | `fabric-skills` |
| Diagnose performance or health issues | `fabric-skills` |
| Build Power BI semantic models, reports, or PBIP workflows | `powerbi-authoring` |

## Power BI Agentic Development (community)

[data-goblin/power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) is a community-maintained plugin marketplace for agent-assisted Power BI and Fabric development. It is separate from Microsoft's Skills for Fabric bundles above.

| Resource detail | Guidance |
|-----------------|----------|
| Purpose | Reusable skills, subagents, and validation hooks for semantic models, DAX, TMDL/PBIP, reports, dashboards, Tabular Editor, and related Fabric workflows. |
| Supported surfaces | Claude Code and GitHub Copilot CLI; check the upstream README for host-specific compatibility and setup. |
| Reusable patterns | Agent-assisted authoring, model and report review, project validation, and repeatable development workflows. |

Use the upstream README for current plugin names, installation, updates, and compatibility notes. Select only the plugins needed for the task; this guide links to the marketplace without copying or installing its contents.

The source repo warns of frequent restructuring and breaking changes. Pin and test a reviewed release for repeatable delivery, and review licensing, hooks, scripts, execution scope, tool permissions, and customer data handling before enabling plugins. Require approval and a rollback path for changes to production assets; plugin validation does not replace human review.

## MCP servers

Skills provide guidance and repeatable agent instructions. MCP servers provide live tool access to data sources, APIs, local files, and Fabric/Power BI operations.

- Use [Fabric MCP resources](../fabric-mcps/README.md) in this repo for Fabric MCP Server, Fabric Core MCP Server, Power BI Modeling MCP Server, best practices, and anti-patterns.
- Use upstream [Skills for Fabric MCP setup](https://github.com/microsoft/skills-for-fabric/tree/main/mcp-setup) when a skill workflow needs MCP configuration details.

## Resource format

Each Fabric skill resource should capture:

1. **Resource name** - The skill, bundle, prompt set, or repo name
2. **Purpose** - What Fabric task or workflow it supports
3. **Supported surfaces** - Copilot CLI, VS Code, Claude Code, Cursor, Windsurf, or other agent surfaces
4. **Reusable patterns** - Which repeatable Fabric motions, scenarios, or architecture tasks it enables
5. **Usage notes** - Any setup, permissions, or operational considerations

## Key principle

> **Skills turn guidance into repeatable action.** Fabric skill resources should help teams move from architecture recommendations to executable workflows that can be reused across customers and engagements.
