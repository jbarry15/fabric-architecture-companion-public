# Anti-Pattern: Shipping Unreviewed Copilot-Generated Code

## What it looks like

- Copilot-generated SQL, KQL, DAX, or PySpark is pasted into production without review
- Trusting a natural-language-to-SQL/KQL result as correct because "Copilot wrote it"
- Auto-applying **Fix with Copilot** suggestions without reading the diff
- Treating Copilot output as authoritative rather than as a draft

## Why it's a problem

- **LLMs can be wrong** — generated queries can return subtly incorrect results, miss edge cases, or misinterpret intent, especially over poorly named or undocumented data.
- **Silent data errors** — an incorrect join or filter can produce plausible-but-wrong numbers that flow into reports and decisions.
- **Performance & cost** — naive generated code can trigger expensive scans, shuffles, or full-table reads.
- **Security/governance** — unreviewed code may bypass intended access patterns or expose data.

## What to do instead

✅ **Treat output as a draft** — always review and test generated SQL/KQL/DAX/code before shipping; validate results against known values.

✅ **Review the approval diff** — on [Fix with Copilot](https://learn.microsoft.com/en-us/fabric/data-engineering/copilot-notebooks-overview) and similar features, read the change before applying it.

✅ **Improve grounding** — clear table/column names, defined relationships, and curated [Gold-layer](../best-practices/medallion-architecture.md) star schemas make Copilot far more accurate. See [Copilot in Fabric best practices](../best-practices/copilot-in-fabric.md).

✅ **Keep humans accountable** — Copilot augments, it doesn't replace review, testing, and governance ownership.

## When trusting output more freely is OK (Silver pattern)

- Exploratory/ad-hoc analysis where results are sanity-checked and not operationalized
- Boilerplate or scaffolding code that is subsequently reviewed and tested
- Learning/prototyping contexts with no production impact

## Severity

🔴 **High** — Unreviewed generated code in production can produce wrong results or unsafe access with no human check.

## References

- [Overview of Copilot in Fabric](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-overview)
- [Privacy, security & responsible use for Copilot](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-privacy-security)
- [Copilot in Fabric best practices](../best-practices/copilot-in-fabric.md)
