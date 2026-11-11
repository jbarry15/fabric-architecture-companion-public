# Keeping This Companion Up to Date

Microsoft Fabric evolves rapidly. This document explains how to maintain the content in this repo so it stays accurate and useful.

## Quick update workflow

```bash
# 1. Pull latest changes
git pull origin master

# 2. Open in VS Code with Copilot agent mode (Ctrl+Shift+I)
#    Ask: "Check if any best practices or anti-patterns are outdated
#          using the Microsoft Learn MCP server"

# 3. Copilot will search live docs and flag discrepancies

# 4. Make updates, commit, and push
git add -A && git commit -m "Update [section] based on latest Fabric docs"
git push
```

## What to check and when

| Frequency | What to review | How |
|-----------|---------------|-----|
| **Monthly** | Anti-patterns — are any now resolved by new features? | Search Fabric release notes |
| **Monthly** | Best practices — any new recommended patterns? | Check Well-Architected Framework updates |
| **Quarterly** | Reference architectures — new deployment patterns? | Review Azure Architecture Center |
| **Quarterly** | Toolbox links — new tools or deprecated ones? | Check [microsoft/fabric-toolbox](https://github.com/microsoft/fabric-toolbox) releases |
| **As needed** | Breaking changes in Fabric | Fabric blog / release notes |

## Using Copilot to help update

### Option 1: VS Code agent mode (recommended)

Open the repo in VS Code, enter agent mode, and ask:

```
Review all anti-patterns in this repo against the latest Microsoft Fabric
documentation. For each one, tell me if it's still accurate or if Fabric
has introduced features that change the guidance.
```

Copilot will use the Microsoft Learn MCP server to search live docs and compare against your content.

### Option 2: Copilot CLI

```bash
cd fabric-architecture-companion

# Check a specific doc
# "Is the Direct Lake best practice doc still accurate?"

# Check for new features that warrant new content
# "What new Fabric features from the last 3 months should be added to this guide?"
```

### Option 3: Manual review with MCP tools

In agent mode, you can be more targeted:

```
Use microsoft_docs_search to find what's new in Fabric Direct Lake
since January 2025. Compare against best-practices/direct-lake.md
and suggest updates.
```

## How to update specific content

### Adding a new anti-pattern

1. Create `anti-patterns/{descriptive-name}.md`
2. Follow this structure:
   ```markdown
   # Anti-Pattern: [Name]
   ## What it looks like
   ## Why it's a problem
   ## What to do instead
   ## When [X] is OK (Silver pattern)   <!-- optional -->
   ## Severity
   ## References
   ```
3. If the anti-pattern maps to an architectural dimension, add/update the matching row in `reference-architectures/patterns-comparison.md` (not every anti-pattern maps to a dimension — skip if it doesn't)
4. Cross-link to the relevant best-practice doc(s)
5. **Bump the count** in the README `### 🚫 Anti-Patterns (N docs)` header

### Adding a new best practice

1. Create `best-practices/{descriptive-name}.md`
2. Follow the current convention used by the docs in this repo:
   ```markdown
   # [Title] Best Practices
   Based on [official Learn link](...) and [supporting link](...)
   ## Overview
   ## Best practices        <!-- grouped, using ✅ / ⚠️ bullets -->
   ## Anti-patterns         <!-- using 🚫 bullets -->
   ## References
   ```
   - **Verify every Learn URL returns 200** before committing (guessed doc URLs often 404).
   - Clearly label preview features and link to the [preview information](https://learn.microsoft.com/en-us/fabric/fundamentals/preview) page.
3. Cross-link from relevant anti-pattern and sibling best-practice docs
4. **Bump the count** in the README `### 📐 Best Practices (N docs)` header and add the file to the `## Structure` tree

### Adding a new workload doc — don't forget these

When you document a whole new Fabric workload, update these shared surfaces too so the repo stays consistent:

- **`README.md`** — section doc count(s) and the `## Structure` tree
- **`.github/instructions/microsoft-learn-mcp.instructions.md`** — add the workload to the **Priority topics to search for** list
- **`anti-patterns/`** — promote the doc's most important inline anti-patterns to standalone docs where they add value

> The **Architecture Checklist** skill reads all of `best-practices/` and `anti-patterns/` automatically, so no skill edit is needed when you add docs to those folders.

### Updating reference architectures

1. Edit the relevant file in `reference-architectures/`
2. If adding images, place them in `reference-architectures/images/`
3. Update the patterns comparison matrix if architectural dimensions change

### Updating toolbox links

1. Check [microsoft/fabric-toolbox](https://github.com/microsoft/fabric-toolbox) for new tools
2. Verify existing links still work (404 check)
3. Add new entries to the appropriate category in `toolbox/README.md`

## Sources to monitor for updates

| Source | URL | What to look for |
|--------|-----|-----------------|
| **Fabric Blog** | https://blog.fabric.microsoft.com/ | New features, GA announcements |
| **Fabric release notes** | https://learn.microsoft.com/en-us/fabric/release-plan/ | Monthly feature updates |
| **Well-Architected Framework** | https://learn.microsoft.com/en-us/azure/well-architected/microsoft-fabric/ | Pillar updates |
| **Azure Architecture Center** | https://learn.microsoft.com/en-us/azure/architecture/ | New reference architectures |
| **Fabric Toolbox repo** | https://github.com/microsoft/fabric-toolbox | New tools and accelerators |
| **Fabric Community** | https://community.fabric.microsoft.com/ | Common customer issues (new anti-patterns) |

## Automating updates (optional)

If you want automated checks, you can add a GitHub Actions workflow or Copilot agent workflow similar to the pattern in [copilot-cli-for-beginners](https://github.com/github/copilot-cli-for-beginners):

```yaml
# .github/workflows/check-updates.md (Copilot agent workflow)
name: "Content Freshness Check"
description: "Weekly check for outdated content against live Fabric docs"
on:
  schedule: weekly
tools:
  web-fetch:
  github:
    toolsets: [repos]
```

This would have a Copilot agent periodically compare your docs against live Microsoft Learn content and open PRs when updates are needed.

## Version tracking

Consider adding a "Last reviewed" date at the bottom of each doc:

```markdown
---
*Last reviewed: 2026-05-28 | Fabric version context: May 2026 release*
```

This helps readers gauge how fresh the content is and helps maintainers prioritize which docs need re-review.
