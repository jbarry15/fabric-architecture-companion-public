# Fabric Capacity Estimator

Source: [Fabric Capacity Estimator](https://www.microsoft.com/en-us/microsoft-fabric/capacity-estimator)

## Quick details

| Field | Detail |
|-------|--------|
| Owner/source | Microsoft Fabric |
| Resource type | Capacity sizing and planning tool |
| Primary area | Fabric pricing and forecasting |
| Related areas | Capacity planning, CU sizing, cost forecasting, workload estimation |
| Status | Reference only |
| Last cataloged | 2026-08-28 |

## What it is

A Microsoft Fabric sizing resource for estimating capacity needs before purchasing or reserving Fabric capacity.

Use it when a customer needs an initial estimate for Fabric capacity sizing based on expected workloads, users, data volumes, concurrency, and usage patterns.

## How to use it with this guide

Use the estimator before recommending a Fabric SKU or Fabric Capacity reservation. Pair the estimate with current pricing, quota checks, regional availability, and observed capacity metrics where available.

## Watch-outs

- Treat the output as a planning estimate, not a guaranteed bill or performance commitment.
- Validate assumptions for workload mix, concurrency, refresh patterns, Spark jobs, Direct Lake usage, and real-time workloads.
- Reconcile estimates with Azure pricing, Cost Management, Fabric capacity metrics, and reservation rules before customer commitment.
