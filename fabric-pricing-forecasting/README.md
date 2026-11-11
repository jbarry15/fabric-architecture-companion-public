# Fabric Pricing and Forecasting

Reusable guidance and artifacts for Microsoft Fabric pricing, capacity planning, consumption forecasting, reservations, and cost-governance scenarios.

## Status

Draft. Initial resources focus on Fabric Capacity reservations and Azure reservation purchase guidance.

## Intended content

| Area | Examples |
|------|----------|
| Pricing guidance | SKU considerations, licensing assumptions, and pricing calculators |
| Capacity forecasting | Workload demand estimates, CU sizing, burst patterns, and scale planning |
| Consumption planning | Forecast inputs, actuals tracking, budget scenarios, and variance analysis |
| Cost governance | Guardrails, monitoring, chargeback/showback, and optimization practices |

## Guides

| Guide | Type | Use it for |
|-------|------|------------|
| [Best practices](best-practices.md) | Guidance | Pricing forecasts, reservation readiness, CU demand modeling, showback/chargeback, and cost-governance patterns |
| [Anti-patterns](anti-patterns.md) | Guidance | Common mistakes that create inaccurate forecasts, reservation waste, budget surprises, and avoidable spend |

## Resources

| Resource | Type | Owner/source |
|----------|------|--------------|
| [Microsoft Fabric Reservations: Understanding What They Are and How to Create Them](resources/microsoft-fabric-reservations-blog.md) | Community blog / field walkthrough | Justin Barry / jbarry15 |
| [Fabric Capacity Estimator](resources/fabric-capacity-estimator.md) | Microsoft sizing tool | Microsoft Fabric |
| [Save costs with Microsoft Fabric Capacity reservations](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/fabric-capacity) | Microsoft Learn | Microsoft Cost Management |
| [Buy an Azure reservation](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/prepare-buy-reservation) | Microsoft Learn | Microsoft Cost Management |
| [Buy Fabric capacity Azure SKUs](https://learn.microsoft.com/en-us/fabric/enterprise/buy-capacity) | Microsoft Learn | Microsoft Fabric |

## Current guidance

- Use reservations for predictable, steady Fabric Capacity usage where the region, scope, and CU quantity can be forecast with confidence.
- Use the Fabric Capacity Estimator as an early sizing input before recommending a SKU or reservation quantity.
- Size reservations by expected CU usage, not just the nominal capacity SKU name.
- Validate quota, region, subscription eligibility, purchasing permissions, and billing scope before recommending a reservation.
- Keep storage, networking, and noncovered charges separate from Fabric capacity reservation savings assumptions.
- Reconcile forecasts with Fabric Capacity Metrics app and Chargeback app data after pilot and production usage is available.
