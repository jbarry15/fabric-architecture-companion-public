---
name: architecture-checklist
description: "Review a Microsoft Fabric architecture description or document against the repository's best practices and anti-patterns."
---

# Architecture Checklist Skill

When invoked, run through the patterns comparison against the user's described architecture.

## Steps

1. Ask the user to describe their current or proposed architecture (or provide a document/diagram)
2. Read the repo's local Markdown guidance first, especially `reference-architectures/patterns-comparison.md`, `anti-patterns/`, and relevant `best-practices/`
3. Use Microsoft Learn only to verify current product behavior, limits, release status, or implementation details
4. For each row in `reference-architectures/patterns-comparison.md`, classify their design as ✅ Gold, ⚖️ Silver, or 🚫 Anti-Pattern
5. For any 🚫 findings, reference the specific anti-pattern doc and suggest the recommended alternative
6. Output a concise summary scorecard showing:
   - Number of Gold / Silver / Anti-Pattern classifications
   - Top 3 priority remediation items

## Context files to read

- `reference-architectures/patterns-comparison.md`
- All files in `anti-patterns/`
- All files in `best-practices/`
