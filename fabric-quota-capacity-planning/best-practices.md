# Fabric Quota and Capacity Planning Best Practices

Use these practices when planning Fabric capacity, quota, workload placement, and growth governance. Treat Microsoft Learn as the source of truth for limits, portal steps, and current feature behavior.

## Core principles

| Principle | Why it matters |
|-----------|----------------|
| Plan by adoption phase | POC, pilot, production, decentralized scale, centralized scale, and growth governance each need different capacity decisions. |
| Validate quota early | Capacity purchase, SKU increases, scale-out, and reservations can fail if the target subscription and region don't have enough available CUs. |
| Measure before scaling | Fabric capacity decisions should be based on observed usage, workload mix, concurrency, throttling, and user experience, not guesswork. |
| Optimize before buying more | Inefficient models, queries, pipelines, notebooks, and refresh patterns create avoidable cost. |
| Balance consolidation and isolation | Shared capacity improves utilization; dedicated capacity protects critical or noisy workloads. |
| Govern continuously | Capacity planning is an operating model, not a one-time sizing exercise. |

## Best practices

### 1. Start with a narrow POC, then scale deliberately

For first deployments, start small with a focused use case, isolated workspace or capacity, limited users, and measurable success criteria. Move from POC to pilot to production only after you have capacity metrics and user feedback.

Use the Fabric Capacity Estimator as an early sizing input, but validate the recommendation with actual telemetry before production.

### 2. Check quota before recommending capacity

Quota determines the maximum CUs available for provisioned Fabric capacities in a subscription and region. You aren't charged for quota itself, but you can't provision capacity beyond the available quota.

Before recommending a new capacity, SKU increase, reservation, or regional deployment:

- Confirm the Azure subscription and target region.
- Register the `Microsoft.Fabric` resource provider if needed.
- Check current usage and quota limit.
- Calculate available quota as `limit - currentValue`.
- Request quota increases early enough to avoid blocking deployment.

### 3. Size from 30-second CU demand

Fabric evaluates capacity usage in 30-second timepoints. Convert SKU size into available CU seconds by multiplying capacity CUs by 30. For example, an F64 provides 64 CUs, or 1,920 CU seconds per 30-second timepoint.

Use the Fabric Capacity Metrics app timepoint views to identify peak CU demand and top consumers. Leave headroom for concurrency, adoption growth, and seasonal spikes.

### 4. Separate development, test, and production

Keep nonproduction workloads away from production capacity when possible. Development, QA, pilot, and stress-test workloads need flexibility and should not compete with business-critical reports, data pipelines, or AI workloads.

Use deployment pipelines, Git integration, and workspace assignment standards so capacity placement is part of release planning.

### 5. Classify workloads by criticality

Classify workloads before assigning capacity:

| Tier | Typical capacity pattern |
|------|--------------------------|
| Tier 1 mission-critical | Dedicated, correctly sized capacity with named admins, alerting, proactive quota path, and SLA review. |
| Tier 2 business operational | Shared or dedicated capacity depending on workload patterns, user impact, and contention risk. |
| Tier 3 ad hoc or sandbox | Consolidated capacity, shared capacity, or small isolated capacity with clear guardrails. |

Capacity tiers should reflect business impact and SLA expectations, not just workload type.

### 6. Use shared capacity where utilization matters

For decentralized self-service analytics, shared capacity can reduce idle capacity and improve cost efficiency across small or medium workloads. Use it when teams have compatible regions, domains, usage patterns, criticality, and ownership expectations.

Pair shared capacity with:

- Fair-use guidelines.
- Capacity Metrics app monitoring.
- Chargeback or showback reporting.
- A clear noisy-neighbor response path.
- COE support for optimization.

### 7. Isolate critical, sensitive, or noisy workloads

Use dedicated capacity when a workload needs stronger performance isolation, clear cost ownership, sensitive-domain separation, or mission-critical SLA support.

Consider scale-out when:

- A workspace or domain repeatedly causes throttling.
- Background processing interferes with interactive reporting.
- Teams need separate admins, settings, budgets, or escalation paths.
- A workload needs reserved headroom.

### 8. Monitor with Capacity Metrics and Real-Time hub events

Use the Fabric Capacity Metrics app as the operational source for capacity health. Review utilization trends, throttling, rejections, operation type, item-level consumption, and timepoint details.

For near-real-time operations visibility, use [Capacity operation events in Real-Time hub](https://learn.microsoft.com/en-us/fabric/real-time-hub/create-streams-fabric-capacity-operation-events) (Preview). These events emit one record for every operation that consumes capacity and include CU consumption, duration, throttling delay, status, workspace, item, operation, and identity context. Route them through Eventstream to Eventhouse, dashboards, or Activator when teams need immediate attribution or alerts for high-CU operations, failures, or throttling.

Do not treat capacity operation events as a replacement for the Capacity Metrics app. Use them as a high-granularity operational stream, and filter or group by fields such as `capacityId`, `workspaceId`, `itemId`, `operationName`, `itemKind`, `utilizationType`, or `status` to avoid noisy alerts on busy capacities.

Operationalize a regular review cadence:

- Daily or weekly for critical capacities.
- Monthly for shared departmental capacities.
- Quarterly for roadmap, budget, quota, and reservation planning.

Account for metrics latency and scheduled refresh behavior when diagnosing recent changes.

Capacity operation event streaming requires capacity admin role on the source capacity and Contributor or higher permissions in the workspace where the eventstream or Activator rule is created. If workspace outbound access protection or workspace-level private links are enabled, validate Real-Time Events connector rules and private-link paths before designing cross-workspace consumption.

### 9. Use chargeback or showback for accountability

Use the Fabric Chargeback app to make usage visible by workspace, item, user, domain, or subdomain. Even when internal billing is not implemented, showback creates accountability and gives teams evidence for optimization or scale requests.

Map workspaces to domains/subdomains so usage isn't buried under "No domain" categories.

### 10. Optimize before scaling up

When capacity is under pressure, first identify the top consumers and optimize them. Then decide whether to scale up or scale out.

Common optimization moves include:

- Tuning DAX, SQL, KQL, and notebook code.
- Reducing unnecessary columns, scans, joins, and refresh frequency.
- Using incremental refresh and partition-aware processing.
- Staggering background workloads where appropriate.
- Fixing inefficient semantic models and warehouse queries.
- Moving experimental or noisy workloads off critical capacity.

### 11. Choose scale-up versus scale-out intentionally

Scale up when a capacity needs more CUs for a compatible group of workloads and shared settings are acceptable. Scale out when isolation, ownership, regional placement, workload thresholds, or noisy-neighbor mitigation matter more than consolidation.

For F SKUs, include pause, resume, and resize options in operating procedures where they fit workload patterns and cost controls.

### 12. Use surge protection as a guardrail, not a sizing strategy

Surge protection can reject background operations sooner to help protect capacity health and interactive experiences. Configure it only after reviewing usage patterns in the metrics app.

Do not treat surge protection as a replacement for correct sizing, optimization, or dedicated capacity for Tier 1 workloads. Document who can enable it, thresholds, affected workloads, and recovery steps.

### 13. Connect quota, capacity, pricing, and reservations

Quota planning, SKU sizing, pricing, and reservations are linked but different:

- Quota is the maximum CUs you can provision.
- Capacity is the Fabric compute you run and pay for.
- Reservation is a commitment that can discount matching capacity usage.
- Pricing depends on SKU, region, billing model, usage, and covered charges.

Use measured steady usage before recommending reservations. Keep storage and networking separate from capacity reservation assumptions.

### 14. Treat F0 and on-demand billing as an adoption accelerator, not a production sizing shortcut

Zero-provisioned F0 and broader on-demand billing were announced as a way to lower the entry barrier for experimentation, demos, and bursty workloads. Use them to help teams start with measured usage rather than speculative SKU sizing, but don't skip production capacity planning.

Apply these guardrails until official pricing, availability, and workload coverage are confirmed for the tenant:

- Use F0/on-demand for POCs, pilots, demos, training, and low-duty-cycle workloads first.
- Move steady, critical, or high-concurrency workloads to intentionally sized capacity once usage patterns are known.
- Keep storage, scan, transaction, Copilot/AI, and workload-specific charges separate in forecasts.
- Validate regional availability, tenant settings, budget alerts, and spending controls before broad self-service enablement.

## Operating checklist

- [ ] Target subscription and region are known.
- [ ] `Microsoft.Fabric` resource provider is registered.
- [ ] Available CU quota is checked and documented.
- [ ] Capacity estimator assumptions are recorded.
- [ ] POC or pilot usage is measured in the Capacity Metrics app.
- [ ] Near-real-time capacity alerting needs are assessed against Capacity operation events in Real-Time hub.
- [ ] Workloads are classified by criticality tier.
- [ ] Shared versus dedicated capacity decision is documented.
- [ ] Dev/test/prod capacity separation is defined.
- [ ] Monitoring cadence and owners are assigned.
- [ ] Chargeback/showback approach is defined.
- [ ] Surge protection stance is documented.
- [ ] Scale-up, scale-out, pause/resume, and support escalation paths are defined.
- [ ] F0/on-demand suitability and spending guardrails are documented for experimental workloads.

## References

- [Plan your Microsoft Fabric capacity](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-overview)
- [Plan your first Microsoft Fabric deployment](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-plan-deployment)
- [Scale capacity for decentralized self-service analytics](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-scale-self-service-analytics)
- [Scale capacity for centralized analytics](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-enterprise-managed-self-service-solutions)
- [Manage capacity growth and governance](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-manage-capacity-growth-governance)
- [Microsoft Fabric quotas](https://learn.microsoft.com/en-us/fabric/enterprise/fabric-quotas?tabs=Azure)
- [Plan your capacity size](https://learn.microsoft.com/en-us/fabric/enterprise/plan-capacity)
- [Evaluate and optimize your Microsoft Fabric capacity](https://learn.microsoft.com/en-us/fabric/enterprise/optimize-capacity)
- [Microsoft Fabric Capacity Metrics app](https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app)
- [Capacity operation events in Fabric Real-Time hub](https://learn.microsoft.com/en-us/fabric/real-time-hub/create-streams-fabric-capacity-operation-events)
- [Explore Fabric capacity operation events](https://learn.microsoft.com/en-us/fabric/real-time-hub/explore-fabric-capacity-operation-events)
- [Set alerts on Fabric capacity operation events](https://learn.microsoft.com/en-us/fabric/real-time-hub/set-alerts-fabric-capacity-operation-events)
- [Microsoft Fabric Chargeback app](https://learn.microsoft.com/en-us/fabric/enterprise/chargeback-app)
- [Understand capacity throttling and smoothing](https://learn.microsoft.com/en-us/fabric/enterprise/throttling)
- [Surge protection](https://learn.microsoft.com/en-us/fabric/enterprise/surge-protection)
