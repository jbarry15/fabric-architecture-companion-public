# Fabric Quota and Capacity Planning Anti-Patterns

These are common mistakes that lead to blocked deployments, avoidable throttling, poor user experience, and unnecessary Fabric spend.

## Anti-patterns

### Quota as an afterthought

**What it looks like**

- Planning a new capacity or SKU increase before checking available CUs.
- Discovering during deployment that the target subscription or region has quota 0 or insufficient quota.
- Assuming quota in one Azure region applies everywhere.

**Why it's a problem**

Capacity creation, scale-up, reservations, and regional expansion can be blocked by quota gaps.

**What to do instead**

Check Fabric quota early by subscription and region. Register the `Microsoft.Fabric` resource provider and request increases before the delivery date.

---

### Sizing by guesswork or budget only

**What it looks like**

- Picking an F SKU because it "sounds right."
- Sizing only from user count or budget.
- Skipping POC/pilot telemetry.

**Why it's a problem**

Fabric consumption depends on workload mix, operation efficiency, concurrency, refresh patterns, Spark jobs, SQL/KQL/DAX behavior, and usage timing.

**What to do instead**

Use the Capacity Estimator for initial planning, then validate with Fabric Capacity Metrics app data and timepoint-level CU analysis.

---

### Treating the estimator as a guarantee

**What it looks like**

- Treating estimator output as a final bill or SLA promise.
- Not documenting assumptions used in the estimate.
- Ignoring region, pricing, workload mix, and user behavior changes.

**Why it's a problem**

Estimator output is approximate. Actual cost and performance depend on real usage, region, pricing, optimizations, and adoption patterns.

**What to do instead**

Record assumptions, validate with telemetry, review Azure pricing, and adjust after pilot or production measurements.

---

### Moving from POC directly to enterprise production

**What it looks like**

- A trial or narrow POC becomes the production pattern without a pilot.
- Development and production share the same capacity.
- No production monitoring, quota path, or escalation model is defined.

**Why it's a problem**

POC success proves feasibility, not enterprise readiness. Production introduces concurrency, governance, reliability, and growth requirements.

**What to do instead**

Move through POC, pilot, and production phases. Separate environments, monitor the pilot, and size production from observed load.

---

### One shared capacity for everything

**What it looks like**

- Mission-critical reports, sandbox notebooks, departmental self-service, heavy refreshes, and experimental AI workloads all share one capacity.
- No fair-use rules or workload placement standards exist.
- Heavy workloads regularly degrade other teams.

**Why it's a problem**

Shared capacity maximizes utilization only when governed. Without isolation and monitoring, noisy neighbors create throttling and inconsistent user experience.

**What to do instead**

Use a hybrid model. Share capacity for compatible noncritical workloads, and isolate critical, sensitive, or heavy workloads onto dedicated capacity when justified.

---

### Dedicated capacity for every team by default

**What it looks like**

- Every department gets its own capacity even when usage is small or intermittent.
- Many capacities sit mostly idle.
- Admin overhead increases without clear SLA benefit.

**Why it's a problem**

Over-isolation increases cost and operational complexity. Idle capacity is still spend unless managed through appropriate pause/resume patterns.

**What to do instead**

Consolidate compatible small and medium workloads. Use chargeback/showback and metrics to decide when a team earns dedicated capacity.

---

### Mixing Tier 1 workloads with uncontrolled ad hoc usage

**What it looks like**

- Executive reporting or regulated enterprise workloads share capacity with experimental notebooks or uncontrolled self-service refreshes.
- No mission-critical workspace exclusions, alerting, or escalation path exists.

**Why it's a problem**

Tier 1 workloads need predictable performance, named ownership, and preplanned recovery paths. Ad hoc work can consume shared compute at the wrong time.

**What to do instead**

Classify workloads by business criticality and isolate Tier 1 workloads with dedicated capacity, monitoring, surge protection stance, and preapproved quota/scale paths.

---

### Scaling before optimizing

**What it looks like**

- Upgrading SKU whenever users report slowness.
- Ignoring inefficient DAX, SQL, KQL, notebooks, refresh schedules, or pipelines.
- No item-level top-consumer review is performed.

**Why it's a problem**

Buying more capacity can hide inefficient content and compound cost. The same bad patterns often consume the larger SKU too.

**What to do instead**

Use the Capacity Metrics app to find high-CU items, optimize content first, then decide whether scale-up or scale-out is still required.

---

### Misreading throttling, bursting, and smoothing

**What it looks like**

- Assuming a short spike means the capacity is undersized.
- Assuming no immediate throttling means the capacity is healthy.
- Ignoring carryforward usage and burndown after overload.

**Why it's a problem**

Fabric uses bursting, smoothing, overage protection, and staged throttling. Capacity health depends on sustained patterns, not one chart point.

**What to do instead**

Review utilization, background/interactive rejection, timepoint details, carryforward, and trends before making capacity decisions.

---

### Treating surge protection as an SLA guarantee

**What it looks like**

- Enabling surge protection and assuming interactive users are fully protected.
- Not documenting which background jobs can be rejected.
- Using surge protection instead of isolating critical workloads.

**Why it's a problem**

Surge protection can reject background work and doesn't guarantee interactive operations avoid delay or rejection. It is a guardrail, not a substitute for right-sizing.

**What to do instead**

Configure surge protection based on measured patterns, document thresholds and impacts, and isolate mission-critical workloads in correctly sized capacity.

---

### No monitoring or cost accountability

**What it looks like**

- Capacity owners don't review metrics.
- Teams don't see their usage.
- Workspaces aren't mapped to domains/subdomains.
- High-consuming items remain invisible until users complain.

**Why it's a problem**

Capacity planning without telemetry becomes reactive. Shared capacity without accountability encourages inefficient usage.

**What to do instead**

Use the Capacity Metrics app for operational monitoring and the Chargeback app for workspace, item, user, domain, or subdomain accountability.

---

### Confusing quota, capacity, cost, and reservations

**What it looks like**

- Treating quota as spend.
- Buying reservations before confirming steady usage.
- Assuming reservations cover storage or networking.
- Recommending reserved CUs without checking scope, region, and actual running capacity.

**Why it's a problem**

Each concept affects planning differently. Confusing them leads to blocked deployments, inaccurate forecasts, or poor reservation utilization.

**What to do instead**

Separate the decisions: quota enables provisioning, capacity runs workloads, pricing estimates cost, and reservations discount matching steady capacity usage.

---

### No COE operating model

**What it looks like**

- No named capacity admin or owner.
- No fair-use policy.
- No review cadence.
- No escalation path for throttling, quota increases, noisy neighbors, or critical workload incidents.

**Why it's a problem**

Capacity planning is continuous. Without governance, usage grows faster than the operating model and cost/performance issues recur.

**What to do instead**

Establish COE-led operating practices for ownership, onboarding, monitoring, chargeback/showback, optimization, scale decisions, and incident response.

## Summary table

| Anti-pattern | Severity | Key fix |
|--------------|:--------:|---------|
| Quota as an afterthought | High | Check subscription/region CU quota early |
| Sizing by guesswork or budget only | High | Measure with estimator plus Capacity Metrics app |
| Treating estimator as a guarantee | Medium | Validate assumptions with telemetry and pricing |
| POC directly to enterprise production | High | Use POC, pilot, then production phases |
| One shared capacity for everything | High | Use hybrid shared/dedicated capacity placement |
| Dedicated capacity for every team | Medium | Consolidate compatible workloads |
| Tier 1 mixed with ad hoc usage | High | Classify and isolate critical workloads |
| Scaling before optimizing | High | Optimize top consumers before buying more |
| Misreading throttling and smoothing | Medium | Review trends, timepoints, and carryforward |
| Surge protection as SLA guarantee | Medium | Treat as guardrail, not right-sizing |
| No monitoring or accountability | High | Use metrics and chargeback/showback |
| Confusing quota/capacity/cost/reservation | High | Separate planning decisions |
| No COE operating model | High | Assign owners, cadence, and escalation |

## References

- [Fabric Quota and Capacity Planning Best Practices](best-practices.md)
- [Plan your Microsoft Fabric capacity](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-overview)
- [Manage capacity growth and governance](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-manage-capacity-growth-governance)
- [Microsoft Fabric quotas](https://learn.microsoft.com/en-us/fabric/enterprise/fabric-quotas?tabs=Azure)
- [Evaluate and optimize your Microsoft Fabric capacity](https://learn.microsoft.com/en-us/fabric/enterprise/optimize-capacity)
- [Understand capacity throttling and smoothing](https://learn.microsoft.com/en-us/fabric/enterprise/throttling)
- [Surge protection](https://learn.microsoft.com/en-us/fabric/enterprise/surge-protection)
