# Fabric Pricing and Forecasting Best Practices

Use these practices when estimating Microsoft Fabric cost, forecasting capacity usage, and evaluating Fabric Capacity reservations. Treat Microsoft Learn, Azure pricing, Cost Management, and live capacity telemetry as the sources of truth before making customer commitments.

## Core principles

| Principle | Why it matters |
|-----------|----------------|
| Separate concepts | Quota, capacity, pricing, reservations, storage, networking, and licenses affect different parts of the forecast. |
| Forecast from CU demand | Fabric capacity spend is driven by Capacity Unit usage, workload efficiency, concurrency, and operating schedule. |
| Use estimates directionally | Calculators and estimators are planning tools, not guaranteed bills or performance commitments. |
| Reserve only predictable usage | Fabric reservations work best for steady capacity usage that can be matched by region, scope, and CU quantity. |
| Reconcile with telemetry | Capacity Metrics and Chargeback app data should refine every forecast after POC, pilot, and production rollout. |
| Govern purchasing centrally | Reservation commitments need approval, ownership, renewal tracking, and utilization review. |

## Best practices

### 1. Build a forecast model before recommending a SKU

Start with expected workloads, users, concurrency, refresh schedules, Spark and data engineering jobs, Direct Lake reporting patterns, real-time workloads, AI workloads, and operating hours. Use the Fabric Capacity Estimator for an initial sizing view, then document the assumptions behind the recommendation.

For existing deployments, replace assumptions with actual Fabric Capacity Metrics app data.

### 2. Keep quota, capacity, pricing, and reservations separate

Use clear definitions in every planning artifact:

| Concept | Use in planning |
|---------|-----------------|
| Quota | Determines how many Fabric CUs can be provisioned in a subscription and region. |
| Capacity | The Fabric compute resource that runs workloads and incurs capacity charges. |
| Pricing | The regional cost model for the selected SKU, billing option, and covered charges. |
| Reservation | A one-year or three-year commitment that can discount matching Fabric capacity usage. |
| Storage/networking | Separate charges that aren't covered by Fabric Capacity reservations. |

This prevents a quota approval, capacity purchase, or reservation commitment from being treated as the same decision.

### 3. Validate purchase readiness early

Before a customer buys capacity or a reservation, confirm:

- Azure subscription and billing account eligibility.
- Built-in Owner or Reservation Purchaser access for reservations.
- Capacity creation permissions for Fabric F SKUs.
- `Microsoft.Fabric` resource provider registration.
- Supported Fabric region.
- Available CU quota in the selected subscription and region.
- Internal approval path for commitments, renewals, and budget ownership.

### 4. Size reservations by steady CU usage

Fabric Capacity reservations are purchased in CU quantities and apply when usage matches reservation attributes. Size reserved CUs against the steady baseline, not the highest spike.

Keep bursty, experimental, seasonal, or still-growing workloads on pay-as-you-go until the usage pattern is stable enough to commit.

### 5. Match reservation scope to cost ownership

Choose the reservation scope based on how the customer wants savings to apply:

| Scope | Best fit |
|-------|----------|
| Resource group | One workload or cost center needs tight reservation coverage. |
| Subscription | Capacity usage is owned and forecast within one subscription. |
| Shared billing context | Multiple eligible subscriptions should consume the benefit. |
| Management group | Enterprise reservation coverage should align to a managed hierarchy. |

Document which capacities are expected to consume the reservation and how changes to billing context, subscription placement, or region affect coverage.

### 6. Include noncovered costs in the forecast

Reservations cover Fabric capacity usage only. Keep these outside the reservation savings calculation:

- OneLake and other storage charges.
- Networking charges.
- Licenses.
- Workloads or usage that don't match the reservation attributes.
- Any non-Fabric Azure services in the architecture.

Power BI app distribution and Fabric app licensing can change the noncovered-cost side of the forecast. Validate whether the scenario is served by Pro, Premium Per User, Fabric capacity, or a preview licensing path before treating capacity as the only cost lever.

### 6a. Model OneLake billing separately from capacity

OneLake storage, table/API transactions, item-size scans, shortcut access patterns, and announced on-demand OneLake compute can behave differently from always-on capacity planning. Keep a separate OneLake line in forecasts:

- Storage is pay-as-you-go and doesn't consume Fabric CUs.
- OneLake data operations consume Fabric capacity units based on operation type.
- Storage reports and item-size scans can consume CUs during scan operations.
- On-demand OneLake billing and zero-provisioned Fabric (F0) lower entry friction, but don't remove the need to forecast storage, transactions, scans, and high-volume access patterns.

Use the Capacity Metrics app storage page and item-level OneLake storage reports to reconcile estimates after pilots.

### 7. Reconcile estimates with Capacity Metrics and Chargeback app data

Use the Fabric Capacity Metrics app to track utilization, throttling, top-consuming items, operation mix, and timepoint demand. Use the Chargeback app to understand which workspaces, users, domains, and items drive consumption.

Update the forecast after major onboarding events, new workload releases, seasonal peaks, optimization work, SKU changes, and capacity assignment changes.

### 8. Optimize before increasing committed spend

Before scaling up or reserving more CUs, identify top consumers and optimize inefficient content. Common fixes include tuning semantic models, refresh patterns, DAX, SQL, KQL, notebooks, pipelines, and data movement.

Commitment discounts are useful only after avoidable waste has been removed.

### 9. Plan renewal, expiration, exchange, and cancellation governance

Reservations don't automatically renew unless renewal is configured. Workloads continue after expiration, but matching usage returns to pay-as-you-go rates.

Track:

- Reservation owner.
- Term and renewal setting.
- Scope and region.
- CU quantity.
- Expected consuming capacities.
- Utilization review cadence.
- Approval path for renewal, exchange, or cancellation.

### 10. Use showback or chargeback for accountability

Connect pricing forecasts to the teams that drive cost. Use workspace/domain tagging, capacity assignment standards, and Chargeback app views to show usage by business area.

Even if internal billing is not implemented, showback improves optimization behavior and helps justify scale or reservation decisions.

### 11. Preserve flexibility for uncertain workloads

Use pay-as-you-go capacity, smaller initial commitments, pause/resume patterns, or short review cycles when adoption is uncertain. Don't commit long-term reserved CUs for workloads that are still in discovery, migration, or heavy optimization.

### 12. Document assumptions and confidence level

Every forecast should identify what is known, estimated, and TBD. Include assumptions for:

- Workload inventory and growth.
- User count and concurrency.
- Data volume and refresh cadence.
- Capacity region and billing model.
- CU baseline and peak demand.
- Optimization backlog.
- Reservation scope and term.
- Excluded costs.

## Operating checklist

- [ ] Capacity Estimator inputs are documented.
- [ ] Azure pricing page and region are reviewed.
- [ ] Subscription, billing, and purchasing permissions are confirmed.
- [ ] Fabric CU quota is checked.
- [ ] Capacity Metrics app data is used where available.
- [ ] Chargeback/showback model is defined.
- [ ] Reservation quantity is based on steady CU usage.
- [ ] Reservation scope, region, term, and renewal approach are documented.
- [ ] Storage, networking, licenses, and noncovered services are excluded from reservation savings.
- [ ] Optimization actions are reviewed before scaling or reserving more CUs.

## References

- [Fabric Capacity Estimator](resources/fabric-capacity-estimator.md)
- [Microsoft Fabric Reservations field walkthrough](resources/microsoft-fabric-reservations-blog.md)
- [Microsoft Fabric pricing](https://azure.microsoft.com/pricing/details/microsoft-fabric/#pricing)
- [Save costs with Microsoft Fabric Capacity reservations](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/fabric-capacity)
- [Buy an Azure reservation](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/prepare-buy-reservation)
- [Buy Fabric capacity Azure SKUs](https://learn.microsoft.com/en-us/fabric/enterprise/buy-capacity)
- [Microsoft Fabric Capacity Metrics app](https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app)
- [Microsoft Fabric Chargeback app](https://learn.microsoft.com/en-us/fabric/enterprise/chargeback-app)
- [OneLake consumption and billing](https://learn.microsoft.com/en-us/fabric/onelake/onelake-consumption)
