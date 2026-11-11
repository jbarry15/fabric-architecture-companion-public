# Fabric Data Agent AI-ready checklist

Use this checklist before building or publishing a Fabric data agent. The goal is not to expose every table to AI; the goal is to give the agent a governed, scoped, business-friendly source it can answer from reliably.

## Gap summary

Use these labels to summarize the missing pieces, not to assign a numeric score.

| Gap status | Use when |
|------------|----------|
| **No blocking gaps found** | Data is curated, governed, scoped to one domain, and tested with known business questions. |
| **Gaps to address** | The data exists, but names, descriptions, security, relationships, or Prep for AI configuration are incomplete. |
| **Blocking gaps found** | Data is raw, fragmented, unowned, poorly secured, or spread across conflicting sources of truth. |

## Platform and data foundation

- [ ] The agent uses governed Fabric sources, preferably curated Gold-layer data products in OneLake.
- [ ] The source is scoped to one business domain or use case.
- [ ] Data owners, support contacts, and refresh expectations are documented.
- [ ] Tables, views, semantic models, KQL functions, or ontology objects use business-friendly names.
- [ ] Descriptions explain business meaning, not only technical lineage.
- [ ] Quality checks cover duplicates, nulls, stale data, and source reconciliation.
- [ ] OneLake catalog metadata, endorsements, sensitivity labels, and domain assignments are current.

## Semantic model readiness

Use semantic models when users need trusted measures, KPIs, hierarchies, RLS/CLS, and curated business metrics.

- [ ] The model follows a star schema with clear facts, dimensions, relationships, and measures.
- [ ] Unused tables, columns, and measures are removed or hidden.
- [ ] Measures use clear names and descriptions that match business language.
- [ ] Descriptions front-load preferred usage, disambiguation, grain, and units in the first 200 characters.
- [ ] Numeric identifiers, year/month numbers, and postal codes are not accidentally summarized.
- [ ] Key dimensions have natural row labels and useful hierarchies for drill-down questions.
- [ ] Calculation groups, if used, describe available calculation items in business language.
- [ ] RLS/CLS/OLS rules are tested with representative users.
- [ ] Performance is acceptable for conversational use.
- [ ] Best Practice Analyzer or equivalent semantic-model checks have been run.

## Prep for AI

For Power BI semantic model sources, put semantic-model-specific AI configuration in **Prep for AI**, not in data agent instructions.

- [ ] Required Copilot tenant/capacity settings and Power BI Q&A prerequisites are enabled or explicitly validated.
- [ ] **AI Data Schema** includes only the tables, columns, and measures the agent should use.
- [ ] Dependent objects are included when selected measures rely on them.
- [ ] Technical columns, ambiguous fields, and irrelevant objects are excluded.
- [ ] **Verified Answers** cover the highest-value recurring questions with natural phrasing variations.
- [ ] **AI Instructions** define metric preferences, business terms, fiscal/calendar logic, and default groupings.
- [ ] The objects selected in the data agent match the AI Data Schema scope.

## Agent scope and instructions

- [ ] The agent has a clear domain, audience, owner, and supported-question boundary.
- [ ] Planned consumption mode is documented: reports only, conversational BI/Copilot, or both.
- [ ] Business process, key metrics, common questions, ambiguous terms, and vocabulary are captured before writing descriptions or instructions.
- [ ] It connects only the sources needed for that domain.
- [ ] It stays within the current source limits and avoids broad "answer anything" scope.
- [ ] Agent-level instructions handle source routing, tone, fallback behavior, and cross-source terminology.
- [ ] Source-specific instructions explain dataset-specific terms and query hints where supported.
- [ ] Unsupported use cases are named explicitly so users know when not to rely on the agent.

## Security and governance

- [ ] The agent uses least-privilege access and respects existing Fabric permissions.
- [ ] User identity passthrough is preferred when user-level authorization must persist.
- [ ] Sensitivity labels, Purview controls, DLP/access restriction policies, and tenant settings are validated.
- [ ] External consumption surfaces are reviewed for data boundary, geography, authentication, and storage behavior.
- [ ] The published agent description is precise enough for MCP clients or orchestrators to invoke it safely.

## Evaluation before rollout

- [ ] A ground-truth question set covers common, ambiguous, edge-case, and out-of-scope questions.
- [ ] Answers are compared against trusted reports, SQL/DAX/KQL queries, or verified business owner responses.
- [ ] Failures are fixed in the right layer: data/model quality, Prep for AI, source instructions, or agent instructions.
- [ ] Runtime choice is documented; preview runtime features are tested separately before production use.
- [ ] Usage, answer quality, and source selection are monitored after release.

## Common missing pieces

| Missing piece | Fix |
|---------------|-----|
| Fragmented or conflicting sources | Consolidate into governed OneLake data products before building the agent. |
| Full semantic model exposed | Configure Prep for AI and select only the relevant schema. |
| Technical names and missing descriptions | Rename or describe objects in business language. |
| Model-specific instructions in the agent | Move semantic model guidance into Prep for AI. |
| Too many sources or domains | Split into focused, single-domain agents. |
| No answer evaluation | Create a ground-truth question set before publishing broadly. |

## References

- [Fabric data agent concept](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent)
- [Add data sources to a data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-add-datasources)
- [Fabric Data Agent semantic model best practices](https://learn.microsoft.com/en-us/fabric/data-science/semantic-model-best-practices)
- [Best practices for configuring your data agent](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-configuration-best-practices)
- [Fabric data agent runtime](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-runtime)
