# Fabric Quota and Capacity Planning

Reusable guidance and artifacts for Microsoft Fabric quota management, capacity planning, capacity unit (CU) sizing, regional availability, and workload growth planning.

## Status

Draft. Initial resources focus on Microsoft Fabric capacity planning, quota management, and quota-aware deployment readiness.

## Intended content

| Area | Examples |
|------|----------|
| Quota planning | CU quota requests, regional limits, subscription constraints, and lead-time planning |
| Capacity sizing | SKU selection, CU demand estimates, workload mix, concurrency, and growth assumptions |
| Capacity operations | Monitoring, utilization review, scale-up/down decisions, and throttling response |
| Governance | Ownership model, approval workflow, chargeback/showback, and capacity assignment standards |

## Resources

| Guide | Type | Use it for |
|-------|------|------------|
| [Best practices](best-practices.md) | Guidance | Quota checks, capacity sizing, workload placement, monitoring, optimization, and governance patterns |
| [Anti-patterns](anti-patterns.md) | Guidance | Common mistakes that cause blocked deployments, throttling, poor isolation, and unnecessary spend |

## Microsoft Learn resources

| Resource | Type | Owner/source |
|----------|------|--------------|
| [Plan your Microsoft Fabric capacity](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-overview) | Microsoft Learn | Microsoft Fabric |
| [Plan your first Microsoft Fabric deployment](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-plan-deployment) | Microsoft Learn | Microsoft Fabric |
| [Scale capacity for decentralized self-service analytics](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-scale-self-service-analytics) | Microsoft Learn | Microsoft Fabric |
| [Scale capacity for centralized analytics](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-enterprise-managed-self-service-solutions) | Microsoft Learn | Microsoft Fabric |
| [Manage capacity growth and governance](https://learn.microsoft.com/en-us/fabric/enterprise/capacity-planning-manage-capacity-growth-governance) | Microsoft Learn | Microsoft Fabric |
| [Microsoft Fabric quotas](https://learn.microsoft.com/en-us/fabric/enterprise/fabric-quotas?tabs=Azure) | Microsoft Learn | Microsoft Fabric |

## Current guidance

- Check Fabric CU quota before recommending new capacities, SKU increases, reservations, or regional expansion.
- Build a capacity roadmap from POC to production to enterprise scale; don't treat first-deployment sizing as the final operating model.
- Balance consolidation and isolation for decentralized teams; use shared capacity for efficiency and dedicated capacity where SLA, criticality, or noisy-neighbor risk justify it.
- Classify centrally managed analytics by business criticality and isolate Tier 1 workloads with dedicated capacity, monitoring, and preapproved quota paths.
- Use POC and pilot telemetry from the Fabric Capacity Metrics app to validate production SKU recommendations.
- Establish a COE-led operating model for fair use, chargeback/showback, monitoring, proactive scaling, and workload optimization.
- Plan capacity by subscription and Azure region; quota in one region doesn't guarantee capacity availability in another.
- Register the `Microsoft.Fabric` resource provider before the first capacity deployment or when quota shows as 0.
- Keep quota availability separate from pricing, billing, and reservation commitments.
