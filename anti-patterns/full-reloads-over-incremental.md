# Anti-Pattern: Full Reloads Instead of Incremental / CDC

## What it looks like

- Ingestion pipelines truncate-and-reload the entire source dataset on every run
- Re-copying all history nightly when only a small fraction of rows changed
- No watermark, change-tracking, or change-data-capture strategy
- Bronze grows by full snapshots instead of appended/merged deltas

## Why it's a problem

- **Wasted compute & cost** — Reprocessing unchanged data consumes CUs and time that scale with total size, not change volume.
- **Longer run times & tighter windows** — Full reloads lengthen as data grows, eventually breaking the batch window.
- **Source load** — Repeatedly extracting entire tables strains operational source systems.
- **No change history** — Truncate-and-reload discards the ability to process only what changed downstream.

## What to do instead

✅ **Use Copy job incremental copy & CDC** — [Copy job](https://learn.microsoft.com/en-us/fabric/data-factory/what-is-copy-job) supports incremental copy and native [change data capture](https://learn.microsoft.com/en-us/fabric/data-factory/cdc-copy-job) so only new/changed rows move.

✅ **Watermark your loads** — track a high-water mark (timestamp/sequence) and pull only rows beyond it.

✅ **Merge, don't reload** — use Delta `MERGE` / change data feed to upsert changes into Bronze/Silver. See [Data Engineering best practices](../best-practices/data-engineering.md).

✅ **Prefer replication for operational sources** — for Fabric-native or mirrorable databases, let [automatic OneLake replication / Mirroring](custom-etl-over-onelake-replication.md) handle continuous change capture.

## When a full reload is OK (Silver pattern)

- Small reference/dimension tables where a full refresh is cheaper than tracking changes
- Initial/seed load, or a one-time backfill
- Sources that expose no reliable change signal (last resort)

## Severity

⚠️ **Medium** — Functional at small scale but increasingly expensive and slow; move to incremental/CDC as volumes grow.

## References

- [What is Copy job?](https://learn.microsoft.com/en-us/fabric/data-factory/what-is-copy-job)
- [Change data capture in Copy job](https://learn.microsoft.com/en-us/fabric/data-factory/cdc-copy-job)
- [Data movement decision guide](https://learn.microsoft.com/en-us/fabric/data-factory/decision-guide-data-movement)
- [Data Factory best practices](../best-practices/data-factory.md)
