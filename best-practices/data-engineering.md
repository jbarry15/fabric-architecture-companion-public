# Best Practice: Data Engineering in Microsoft Fabric

Based on the [Fabric Spark Best Practices](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-best-practices-overview), [Lakehouse documentation](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-overview), and [Data Engineering docs](https://learn.microsoft.com/en-us/fabric/data-engineering/)

## Overview

Fabric Data Engineering enables users to design, build, and maintain data systems using Apache Spark, Lakehouses, Notebooks, and Pipelines. The core compute engine is Spark running on Delta Lake within OneLake, providing ACID transactions, schema enforcement, and time travel.

## Core components

| Component | Purpose |
|-----------|---------|
| **Lakehouse** | Unified storage combining data lake flexibility with warehouse structure (Delta tables + Files) |
| **Notebooks** | Interactive Spark development (PySpark, Spark SQL, Scala, R) |
| **Spark Job Definitions** | Scheduled/production Spark jobs |
| **Environments** | Manage Spark runtime, libraries, and configurations |
| **Custom live pools** | Pre-warmed notebook Spark pools for fast interactive or scheduled notebook startup windows |
| **Data Pipelines** | Orchestration and ETL workflows |
| **Dataflows Gen2** | Low-code data preparation (Power Query) |
| **Data engineering agent (Project Osmos)** | Preview autonomous task capability that plans, executes, and validates bounded data-engineering outcomes against authorized Lakehouse resources |

## Best practices

### Spark development

| Practice | Details |
|----------|---------|
| **Use a current supported runtime** | [Runtime 2.0](https://learn.microsoft.com/en-us/fabric/data-engineering/runtime-2-0) is GA and the default for new workspaces and Environment items; it includes Spark 4.1, Delta Lake 4.2, Python 3.13, and Native Execution Engine support. Validate libraries, settings, and workloads before upgrading existing environments, then publish and test changes |
| **Use Native Execution Engine where it fits** | Enable Fabric's native/vectorized execution path for supported Spark workloads, especially JSON-heavy ingestion and transformation, but validate fallback behavior for unsupported operators |
| **Prefer DataFrames over RDDs** | DataFrames use the Catalyst optimizer and Tungsten engine; RDDs bypass all optimizations |
| **Enable Adaptive Query Execution (AQE)** | Dynamically optimizes shuffle partitions and handles skewed data automatically |
| **Use Delta for managed tables** | Use Delta for curated Lakehouse tables and serving layers; raw Files/shortcuts can preserve original source format when fidelity or no-copy access matters |
| **Apply column pruning and filtering early** | Select only needed columns and filter rows before joins to reduce shuffle and memory |
| **Avoid `collect()` and `toPandas()` on large data** | Pulls all data to driver memory — causes OOM errors |
| **Use Pandas UDFs over Python UDFs** | Vectorized Arrow-based transfer; 10-100× faster than row-by-row Python UDFs |
| **Use built-in functions first** | DataFrame built-in functions run natively on JVM; avoid UDFs when a built-in exists |
| **Use broadcast joins for small tables** | Avoids expensive shuffles; ideal for dimension tables < 100 MB |
| **Use `log4j` instead of `print()`** | `print()` burdens the driver; `log4j` integrates with Spark diagnostics |

### Notebook best practices

| Practice | Details |
|----------|---------|
| **Place `%%configure` in the first cell** | Ensures Spark session initializes with correct settings before code runs |
| **Parameterize notebooks** | Use parameters for environment-specific values (lakehouse names, paths) |
| **Use Variable Libraries for CI/CD** | Dynamically inject lakehouse names, mount points, configs per environment |
| **Wrap operations in try/except** | Use structured error handling with `logger.error` for exceptions |
| **Use High Concurrency mode for dev** | Share sessions across notebooks to reduce startup time and optimize costs |
| **Use global temp views in HC sessions** | Share DataFrames across notebooks: `df.createOrReplaceGlobalTempView("view_name")` |
| **Store JARs in Lakehouse** | Pass via `spark.jars` instead of uploading to environment (faster publish times) |
| **Use Copilot completions** | Type queries as `#` comments for inline AI code suggestions |

### AI-assisted engineering

| Practice | Details |
|----------|---------|
| **Use Copilot and agents as accelerators** | Let Copilot generate, explain, refactor, or fix notebook and Spark code, but keep code review, tests, and lineage the same as human-authored changes |
| **Ground assistance in repo/workspace context** | Clear notebook names, lakehouse bindings, variable libraries, schemas, and comments make generated code more accurate |
| **Review generated transformations** | Validate filters, joins, null handling, incremental logic, and write modes before committing or scheduling |
| **Use Project Osmos for bounded Fabric-hosted outcomes** | Data engineering agent tasks can inspect data, create/test notebooks, transform data, write Delta tables, and validate results in Fabric, but they remain preview and should be reviewed before production use |
| **Review Project Osmos task settings before execution** | Confirm permission boundaries, writable resources, rerun semantics, schema evolution, artifact destinations, and promote strategy before allowing an agent to change Lakehouse data |
| **Use notebook agent/toolkit features as accelerators** | Let agentic notebook tooling help generate, explain, or troubleshoot code, but keep deterministic tests and review gates for production jobs |

Project Osmos is a fit for outcome-oriented data engineering work such as medallion pipelines, table integration, data quality remediation, migration assistance, and performance tuning where the agent can validate outputs in the target Lakehouse. It is not the right surface for quick ad-hoc queries, Power BI report changes, or unattended production ETL without promotion into governed notebooks, pipelines, monitoring, and source control.

### Delta table optimization

| Practice | Details |
|----------|---------|
| **Use V-Order selectively** | Enable V-Order for read-heavy Silver/Gold tables consumed by Power BI, Direct Lake, or SQL engines; don't apply it blindly to write-heavy Bronze/staging tables |
| **Use adaptive target file size** | Prefer Fabric's adaptive target file size; override `delta.targetFileSize` only for specific tuning or test scenarios |
| **Optimize file sizes** | Use `OPTIMIZE`, optimize write, and auto compaction to prevent excessive small files while preserving enough parallelism |
| **Use Z-ORDER on filter columns** | Co-locate related data for faster reads on commonly filtered columns |
| **Partition large tables wisely** | Partition by low-cardinality columns (date, region); avoid over-partitioning |
| **Run VACUUM regularly** | Remove old files outside retention period to reclaim storage |
| **Use Lakehouse Maintenance activity** | Automate Optimize/Vacuum as a pipeline step when table maintenance should be part of orchestration |
| **Provide schema explicitly** | Never rely on schema inference for production reads of JSON/XML — use predefined schemas |
| **Use Delta MERGE for upserts** | Single-pass insert/update/delete; more efficient than read-modify-write |
| **Enable change data feed** | Track row-level changes for incremental processing downstream |

### Lakehouse organization

| Practice | Details |
|----------|---------|
| **Follow medallion architecture** | Bronze (raw) → Silver (cleansed) → Gold (business-ready) |
| **Separate Lakehouses per layer** | `lh_bronze`, `lh_silver`, `lh_gold` for clear ownership and access control |
| **Use schemas for logical grouping** | Organize tables within a Lakehouse using schemas |
| **Use Tables for structured data** | Delta tables in the `Tables/` section for queryable, governed data |
| **Use Files for unstructured/landing** | Raw files in `Files/` before processing into Delta tables |
| **Centralize Materialized Lake Views** | Create all MLVs for a workspace in a single Lakehouse for clear DAG visibility |
| **Naming conventions** | `snake_case` for tables, prefix with domain (`sales_orders`, `hr_employees`) |

### Capacity and cluster planning

| Practice | Details |
|----------|---------|
| **Right-size clusters** | Match executor count and memory to workload; don't over-provision |
| **Use custom live pools for latency-sensitive notebooks** | Pre-warm notebook compute during known development or scheduled-run windows when startup latency is material; avoid always-warm pools for sporadic workloads |
| **Use autoscale for variable workloads** | Scale up for peak processing, scale down when idle |
| **Monitor CU consumption** | Track capacity unit usage to avoid throttling |
| **Use separate capacity for dev/test** | Prevent development workloads from impacting production |
| **Benchmark before production** | Profile workloads to determine optimal partition count and cluster size |

### Monitoring and observability

| Practice | Details |
|----------|---------|
| **Configure Diagnostic Emitter** | Send Spark event logs and metrics to Azure Log Analytics, Storage, or Event Hubs |
| **Use Spark UI for skew detection** | Check task duration distribution (mean vs max) in the Jobs UI |
| **Check shuffle read/write sizes** | Large per-partition shuffles indicate skew or poor partitioning |
| **Capture failed records** | Write bad rows to a separate Lakehouse table for debugging |
| **Monitor session startup times** | Use HC mode or pipeline session tags if startup latency is too high |

## Anti-patterns

| Anti-pattern | Impact | Fix |
|---|---|---|
| **Using `collect()` on large datasets** | Driver OOM crash; job failure | Use `take()`, `limit()`, or write results to Delta table |
| **Relying on schema inference in production** | Slow reads, memory spikes, schema drift | Define and persist schema explicitly |
| **Using Python UDFs for row-by-row logic** | JVM-to-Python serialization per row; extreme slowness | Use Pandas UDFs (vectorized) or built-in functions |
| **Using RDDs instead of DataFrames** | Bypasses Catalyst optimizer; no predicate pushdown | Always prefer DataFrame API |
| **Too many small files (small file problem)** | Excessive metadata, slow reads, poor parallelism | Run `OPTIMIZE` regularly; batch writes appropriately |
| **Over-partitioning** | Too many small files per partition; excessive file listing | Partition only on low-cardinality columns; evaluate if needed |
| **No data skew handling** | One task takes 100× longer; job timeout | Use AQE, key salting, broadcast joins, or repartition |
| **Hardcoded paths and connection strings** | Breaks across environments; not CI/CD compatible | Use Variable Libraries, parameters, or `%%configure` |
| **Running everything on a single large cluster** | Wasted capacity; all workloads contend for resources | Right-size per workload; use multiple environments |
| **Keeping live pools warm all day by default** | Avoidable cost for intermittent notebook usage | Schedule live windows around real interactive or scheduled workload demand |
| **No error handling in notebooks** | Silent failures; corrupt data propagates downstream | Wrap in try/except; log errors with `log4j` |
| **Using `print()` for production logging** | Burdens driver; no structured log management | Use `log4j` with named loggers; integrate with Diagnostic Emitter |
| **Skipping VACUUM on Delta tables** | Unbounded storage growth; slow file listing | Schedule regular `VACUUM` outside retention window |
| **Not using V-Order for BI-consumed tables** | Suboptimal Direct Lake performance; slower report queries | Apply V-Order on Gold layer tables consumed by Power BI |
| **Hard-coding file size targets everywhere** | Brittle tuning as table sizes change | Prefer adaptive target file size; override only for known edge cases |
| **Letting preview agents write broadly without guardrails** | Preview agent tasks can create or modify notebooks, tables, and files in authorized resources; broad write scope increases blast radius | Use narrow Lakehouse/workspace scope, review Project Osmos safety settings, prefer staging/clone-and-promote for important data, and move validated artifacts into source control and pipelines |
| **Loading entire dataset to transform a subset** | Wasted compute; slow jobs; OOM risk | Filter and select columns early; use predicate pushdown |
| **Storing structured data in Files/ instead of Tables/** | No schema enforcement, no SQL endpoint access, no governance | Ingest to Delta tables for queryable, governed data |
| **Same Lakehouse for all medallion layers** | Unclear ownership; can't apply different access controls per layer | Separate Lakehouse per layer (bronze, silver, gold) |
| **No incremental processing** | Full reprocessing every run; expensive and slow | Use watermarks, change data feed, or merge-based incrementals |
| **Ignoring Spark UI during development** | Skew, shuffle, and OOM issues go unnoticed | Check Spark UI after every development run; profile stages |

## Memory and performance tuning

### Executor memory breakdown

```
┌─────────────────────────────────────────────┐
│           Executor Memory (e.g., 56 GB)      │
├─────────────────────────────────────────────┤
│  Reserved Memory (JVM internals)             │
├─────────────────────────────────────────────┤
│  User Memory (UDFs, local vars, structures)  │
├─────────────────────────────────────────────┤
│  Storage Memory ←→ Execution Memory          │
│  (cached data)      (shuffles, joins, sorts) │
│                                              │
│  ↕ Dynamic boundary (Spark borrows between)  │
├─────────────────────────────────────────────┤
│  Spill to disk (when memory exceeded)        │
└─────────────────────────────────────────────┘
```

### Key tuning parameters

| Parameter | Purpose |
|-----------|---------|
| `spark.memory.fraction` | Fraction of heap for execution + storage (default 0.6) |
| `spark.memory.storageFraction` | Fraction of memory.fraction for storage (default 0.5) |
| `spark.sql.shuffle.partitions` | Number of partitions for shuffles (default 200; tune to data size) |
| `spark.sql.adaptive.enabled` | Enable AQE for dynamic optimization |
| `spark.sql.autoBroadcastJoinThreshold` | Max size for auto-broadcast (default 10 MB) |

## Data skew mitigation strategies

| Strategy | When to use |
|----------|-------------|
| **AQE skew-join optimization** | Default first approach; handles most moderate skew automatically |
| **Key salting** | Hot keys dominate a join; split hot key into N sub-keys, join, then union |
| **Broadcast join** | Small table (< 100 MB) joined to large table |
| **Repartition** | Unbalanced partitions before an expensive stage |
| **Pre-aggregate** | Reduce cardinality before joining on high-cardinality keys |
| **Filter out nulls** | Null keys create massive skew; handle separately |

## Pipeline orchestration patterns

| Pattern | When to use |
|---------|-------------|
| **Data Pipeline + Notebooks** | Complex transformations with Spark; pipeline handles scheduling and dependencies |
| **Data Pipeline + Copy Activity** | Simple data movement (no transformation) |
| **Dataflows Gen2** | Low-code transformations for citizen developers |
| **Notebook with `mssparkutils.notebook.run()`** | Chain notebooks programmatically within Spark |
| **High Concurrency + Session Tags** | Foreach pipelines with many notebooks sharing config |

## References

### Core documentation
- [Fabric Data Engineering documentation](https://learn.microsoft.com/en-us/fabric/data-engineering/)
- [Spark Best Practices overview](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-best-practices-overview)
- [Runtime 2.0 in Fabric](https://learn.microsoft.com/en-us/fabric/data-engineering/runtime-2-0)
- [Native execution engine overview](https://learn.microsoft.com/en-us/fabric/data-engineering/native-execution-engine-overview)
- [Custom live pools overview](https://learn.microsoft.com/en-us/fabric/data-engineering/custom-live-pools-overview)
- [Configure custom live pools](https://learn.microsoft.com/en-us/fabric/data-engineering/custom-live-pools-configure)
- [Fabric data engineering agent (Project Osmos) overview](https://learn.microsoft.com/en-us/fabric/data-engineering/data-engineering-agent-overview)
- [Get started with Fabric data engineering agent (Project Osmos)](https://learn.microsoft.com/en-us/fabric/data-engineering/data-engineering-agent-get-started)
- [Microsoft Fabric preview information](https://learn.microsoft.com/en-us/fabric/fundamentals/preview)
- [Spark Best Practices: Basics](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-best-practices-basics)
- [Development and Monitoring](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-best-practices-development-monitoring)
- [Capacity and Cluster Planning](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-best-practices-capacity-planning)
- [Spark Security](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-best-practices-security)

### Lakehouse
- [What is a Lakehouse?](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-overview)
- [Create a Lakehouse](https://learn.microsoft.com/en-us/fabric/data-engineering/create-lakehouse)
- [Load data with notebooks](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-notebook-load-data)
- [Lakehouse end-to-end tutorial](https://learn.microsoft.com/en-us/fabric/data-engineering/tutorial-lakehouse-introduction)

### Notebooks and development
- [How to use notebooks](https://learn.microsoft.com/en-us/fabric/data-engineering/how-to-use-notebook)
- [Develop, execute, and manage notebooks](https://learn.microsoft.com/en-us/fabric/data-engineering/author-execute-notebook)
- [Notebook source control](https://learn.microsoft.com/en-us/fabric/data-engineering/notebook-source-control-deployment)
- [Variable Library in notebooks](https://learn.microsoft.com/en-us/fabric/cicd/variable-library/variable-library-overview)

### Delta Lake and optimization
- [Cross-workload table maintenance and optimization](https://learn.microsoft.com/en-us/fabric/fundamentals/table-maintenance-optimization)
- [Delta Lake table optimization](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-table-maintenance)
- [V-Order optimization](https://learn.microsoft.com/en-us/fabric/data-engineering/delta-optimization-and-v-order)
- [Tune Delta table file size](https://learn.microsoft.com/en-us/fabric/data-engineering/tune-file-size)
- [Lakehouse Maintenance activity](https://learn.microsoft.com/en-us/fabric/data-factory/lakehouse-maintenance-activity)

### Monitoring
- [Monitor Spark applications](https://learn.microsoft.com/en-us/fabric/data-engineering/browse-spark-applications-monitoring-hub)
- [Diagnostic Emitter for Log Analytics](https://learn.microsoft.com/en-us/fabric/data-engineering/azure-fabric-diagnostic-emitters-log-analytics)
