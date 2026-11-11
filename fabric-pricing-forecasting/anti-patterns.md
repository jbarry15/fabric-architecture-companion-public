# Fabric Pricing and Forecasting Anti-Patterns

These are common mistakes that create inaccurate forecasts, poor reservation utilization, budget surprises, and avoidable Fabric capacity spend.

## Anti-patterns

### Treating a reservation as capacity planning

**What it looks like**

- Buying reserved CUs before validating workload demand.
- Using reservation availability as the reason to pick a capacity size.
- Treating a discount commitment as proof that the architecture is right-sized.

**Why it's a problem**

A reservation only changes the price of matching capacity usage. It doesn't create better workload design, more accurate sizing, or better performance isolation.

**What to do instead**

Plan capacity first, validate steady CU demand, then decide whether a reservation fits the stable baseline.

---

### Forecasting from user count alone

**What it looks like**

- Estimating cost from the number of viewers, developers, or workspaces.
- Ignoring refresh frequency, data volume, Spark jobs, Direct Lake models, notebooks, pipelines, and concurrency.

**Why it's a problem**

Fabric capacity consumption is workload-driven. A small user population can run expensive workloads, while a large audience can consume efficiently designed shared content.

**What to do instead**

Forecast from workload inventory, CU demand, concurrency, operation mix, and Capacity Metrics app data where available.

---

### Treating the Capacity Estimator as a bill

**What it looks like**

- Sharing estimator output as a guaranteed monthly price.
- Omitting the assumptions behind the estimate.
- Not reconciling with Azure pricing, region, quota, and measured consumption.

**Why it's a problem**

The estimator is directional. Actual cost depends on region, SKU, billing model, workload mix, optimization, utilization, and noncovered charges.

**What to do instead**

Use the estimator to start the conversation, then validate with official pricing, capacity telemetry, and documented assumptions.

---

### Reserving for peak instead of baseline

**What it looks like**

- Buying reservations for the largest observed spike.
- Reserving for seasonal highs without a steady matching workload.
- Ignoring pause/resume or pay-as-you-go options for bursty use.

**Why it's a problem**

Unused reserved capacity in an hour doesn't carry forward. Overcommitting can reduce flexibility and waste spend.

**What to do instead**

Reserve for predictable steady CU usage. Leave volatile, bursty, or seasonal demand on pay-as-you-go until the pattern stabilizes.

---

### Ignoring reservation scope and region

**What it looks like**

- Buying a reservation without confirming where matching capacity usage runs.
- Moving subscriptions or capacities without checking reservation coverage.
- Assuming a shared scope covers every environment.

**Why it's a problem**

Reservation discounts apply only to usage that matches the reservation attributes and eligible billing context.

**What to do instead**

Document region, scope, expected consuming capacities, billing context, and ownership before purchase.

---

### Assuming reservations cover all Fabric costs

**What it looks like**

- Applying reservation savings to storage, networking, licenses, or unrelated Azure services.
- Presenting total architecture cost as if every charge is reservation-discounted.

**Why it's a problem**

Fabric Capacity reservations cover Fabric capacity usage only. Other costs still need separate forecasting and governance.

**What to do instead**

Separate capacity usage from storage, networking, licenses, and non-Fabric services in every forecast.

---

### Buying before quota and permissions are confirmed

**What it looks like**

- Recommending a capacity or reservation without checking subscription eligibility.
- Discovering late that the buyer lacks Owner or Reservation Purchaser access.
- Missing `Microsoft.Fabric` provider registration, regional availability, or CU quota.

**Why it's a problem**

Purchase or provisioning can be delayed even when the business case is approved.

**What to do instead**

Validate subscription, billing, permission, provider registration, region, and quota readiness before presenting a committed plan.

---

### Scaling or reserving before optimization

**What it looks like**

- Increasing SKU or reserved CUs because reports are slow or throttling occurs.
- Not identifying top-consuming items.
- Leaving inefficient models, queries, notebooks, pipelines, and refresh schedules untouched.

**Why it's a problem**

More capacity can mask inefficient design and lock higher spend into the forecast.

**What to do instead**

Use the Capacity Metrics app to find top consumers, optimize first, then adjust SKU or reservation coverage.

---

### No chargeback or showback model

**What it looks like**

- Shared capacity cost is split evenly across teams regardless of usage.
- Workspaces aren't mapped to domains or owners.
- High-consuming teams don't see their impact.

**Why it's a problem**

Flat allocation hides cost drivers and makes forecasts harder to defend.

**What to do instead**

Use Chargeback app views and workspace/domain ownership to show actual usage by team, item, user, or domain.

---

### Forgetting renewal and expiration

**What it looks like**

- No one owns reservation renewal.
- Expiration dates aren't tracked.
- Auto-renewal settings aren't reviewed.
- Finance discovers pay-as-you-go charges after expiration.

**Why it's a problem**

When a reservation expires, workloads keep running but matching usage returns to pay-as-you-go rates unless a replacement reservation is purchased.

**What to do instead**

Track reservation owner, term, renewal setting, utilization, and approval milestones.

---

### Treating pay-as-you-go as always worse

**What it looks like**

- Pushing every customer toward reservations immediately.
- Ignoring uncertainty in adoption, migration, workload optimization, or seasonality.

**Why it's a problem**

Pay-as-you-go preserves flexibility while usage is unstable. Early reservations can lock in the wrong quantity, scope, or region.

**What to do instead**

Use pay-as-you-go for discovery, burst, and uncertain growth. Reserve only the measured baseline.

---

### Mixing pricing guidance with licensing assumptions

**What it looks like**

- Capacity pricing is presented without clarifying user licensing requirements.
- Power BI/Fabric license assumptions are hidden inside capacity cost.
- Customers assume all users and workloads are covered by the capacity line item.

**Why it's a problem**

Capacity, user licensing, and workload eligibility are related but separate planning inputs.

**What to do instead**

Separate capacity forecast, license assumptions, and workload access requirements in the pricing model.

## Summary table

| Anti-pattern | Severity | Key fix |
|--------------|:--------:|---------|
| Reservation as capacity planning | High | Size capacity first, reserve steady baseline second |
| Forecasting from user count alone | High | Model workload CU demand and concurrency |
| Estimator as a bill | Medium | Treat estimates as directional and validate |
| Reserving for peak | High | Reserve predictable steady usage only |
| Ignoring scope and region | High | Match reservation attributes to actual usage |
| Assuming reservations cover all costs | High | Separate capacity from storage, networking, and licenses |
| Buying before quota and permissions | High | Confirm eligibility and provisioning readiness |
| Scaling before optimization | High | Optimize top consumers before buying more |
| No chargeback/showback | Medium | Use usage-based accountability |
| Forgetting renewal/expiration | Medium | Track ownership, term, and renewal |
| Pay-as-you-go is always worse | Medium | Preserve flexibility for uncertain workloads |
| Mixing pricing and licensing | Medium | Document capacity and license assumptions separately |

## References

- [Fabric Pricing and Forecasting Best Practices](best-practices.md)
- [Fabric Capacity Estimator](resources/fabric-capacity-estimator.md)
- [Microsoft Fabric Reservations field walkthrough](resources/microsoft-fabric-reservations-blog.md)
- [Microsoft Fabric pricing](https://azure.microsoft.com/pricing/details/microsoft-fabric/#pricing)
- [Save costs with Microsoft Fabric Capacity reservations](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/fabric-capacity)
- [Buy an Azure reservation](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/prepare-buy-reservation)
- [Microsoft Fabric Capacity Metrics app](https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app)
- [Microsoft Fabric Chargeback app](https://learn.microsoft.com/en-us/fabric/enterprise/chargeback-app)
