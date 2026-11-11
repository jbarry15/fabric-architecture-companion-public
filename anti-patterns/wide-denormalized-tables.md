# Anti-Pattern: Wide Denormalized "Everything Tables"

## What it looks like

- Single tables with 50-200+ columns combining fact and dimension attributes
- One massive table serving all reporting needs ("just join everything into one flat table")
- No separation between measures (facts) and descriptive attributes (dimensions)

## Why it's a problem

- **Ambiguous grain** — Measures, dimensions, and attributes blur together, making results hard to reason about
- **Model inefficiency** — High-cardinality and duplicated attributes reduce compression and make the semantic model harder to govern, even though columnar engines only load needed columns
- **Unusable for analysts** — Hard to navigate hundreds of columns; relationships unclear
- **Conflicts with dimensional modeling** — Star schema (facts + dimensions) is specifically recommended for Direct Lake and Power BI
- **Update complexity** — Changing one dimension attribute requires reprocessing the entire wide table

## What to do instead

✅ **Star schema in Gold layer:**

```
┌──────────────┐     ┌──────────────┐
│ dim_customer │────→│              │
└──────────────┘     │  fact_sales  │
┌──────────────┐     │              │
│ dim_product  │────→│              │
└──────────────┘     └──────────────┘
┌──────────────┐           ↑
│  dim_date    │───────────┘
└──────────────┘
```

- **Fact tables** — Numeric measures, foreign keys, timestamps
- **Dimension tables** — Descriptive attributes, slowly-changing dimensions
- **Relationships** — Defined in the semantic model, enabling flexible slicing

✅ **Benefits:**
- Faster queries (only relevant columns scanned)
- Easier maintenance (update dimensions independently)
- Better user experience (clear, navigable model)

## Severity

🚫 **High** — Degrades performance and conflicts with recommended modeling practices.

## References

- [Star schema design](https://learn.microsoft.com/en-us/power-bi/guidance/star-schema)
- [Power BI Semantic Models](https://learn.microsoft.com/en-us/power-bi/connect-data/service-datasets-understand)
