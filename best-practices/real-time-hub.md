# Real-Time Hub and Business Events Best Practices

Based on [Real-Time hub overview](https://learn.microsoft.com/en-us/fabric/real-time-hub/real-time-hub-overview) and [Business events overview](https://learn.microsoft.com/en-us/fabric/real-time-hub/business-events/business-events-overview).

> **Generally available:** [Business Events reached general availability in September 2026](https://learn.microsoft.com/en-us/fabric/fundamentals/whats-new). Check support for each publisher and consumer separately. For example, an [Eventstream consumer](https://learn.microsoft.com/en-us/fabric/real-time-hub/add-source-business-events#prerequisites) must be in the same region as the event schema set.

## Overview

Real-Time hub is the tenant-wide Fabric entry point for data in motion. It centralizes discovery, ingestion, management, and action over streams, Fabric events, Azure events, CDC feeds, KQL tables, and business events.

Use Real-Time hub when teams need to discover existing streams, create new streaming ingestion flows, react to Fabric or Azure events, or model business events as reusable event contracts across Fabric.

## Real-Time hub capabilities

| Capability | Use it for |
|------------|------------|
| **Stream discovery** | Find streams and KQL tables users can access across the tenant |
| **Source onboarding** | Add sources such as Event Hubs, Service Bus, IoT Hub, Kafka, Kinesis, Google Pub/Sub, MQTT, CDC databases, Fabric events, and Azure Blob events |
| **Stream processing** | Route streams into Eventstream transformations, KQL databases, Eventhouse, Activator, dashboards, and downstream actions |
| **Fabric events** | React to Fabric capacity, workspace item, OneLake, and job events |
| **Business events** | Publish meaningful business state changes with shared schemas and route them to consumers |
| **Alerting and action** | Use Activator, notebooks, user data functions, Spark jobs, Dataflows Gen2, Power Automate, and Eventhouse consumers |

## Business events pattern

Business events are not raw telemetry. They represent meaningful changes that downstream teams should understand and act on, such as `PaymentFailed`, `ShipmentDelayed`, or `DemandForecastDeviationDetected`.

| Event type | Examples | Preferred handling |
|------------|----------|--------------------|
| **Business event** | Order approved, shipment delayed, refund issued, forecast deviation detected | Publish with a governed schema, route through Real-Time hub, store/analyze in Eventhouse, and trigger actions through Activator or functions |
| **Telemetry or diagnostics** | Temperature reading, CPU metric, disk error, raw clickstream event | Ingest as streaming data through Eventstream/Eventhouse; promote to a business event only after logic determines that a meaningful state change occurred |

## Best practices

### Hub and stream governance
- ✅ **Use Real-Time hub as the tenant stream catalog** — help teams discover existing streams and KQL tables before they create duplicate ingestion paths.
- ✅ **Endorse reusable streams** — treat important streams as governed data products with owners, descriptions, access policies, and quality expectations.
- ✅ **Route through Eventstream for processing** — apply filtering, field management, joins/unions, and aggregations before landing or alerting where transformation is needed.
- ✅ **Use source-specific connectors** — prefer built-in CDC, Kafka, Event Hubs, Service Bus, IoT Hub, Fabric events, and Azure events connectors over custom polling.
- ✅ **Prefer mirrored database change feed over duplicate CDC** — when an operational source is already mirrored into Fabric, use the mirrored database change feed connector to turn source changes into reusable streams.
- ✅ **Use custom connectors sparingly** — upload custom/open-source streaming connectors only when built-in sources don't fit, and document ownership, versioning, and support boundaries.
- ✅ **Keep Fabric operational events scoped** — capacity, job, workspace item, and OneLake events can be high volume; filter by capacity, workspace, item, status, or operation before alerting.

### Business event design
- ✅ **Model business events around outcomes** — publish events when business state changes, not for every raw measurement.
- ✅ **Use the schema registry as the contract** — standardize event names, fields, types, semantic meaning, and versioning before broad reuse.
- ✅ **Make Eventstreams schema-aware where event shape matters** — use schemas to separate raw ingestion flexibility from governed downstream contracts.
- ✅ **Separate publishers from consumers** — let publishers emit events once and allow Activator, Eventhouse, notebooks, functions, Power Automate, or downstream processes to subscribe independently.
- ✅ **Use the default Eventhouse for analysis** — Fabric automatically stores published business events there. Verify access and retention before using it for historical analysis, dashboards, troubleshooting, or AI/ML context.
- ✅ **Preview with observability** — use data preview and Eventhouse queries to confirm payload shape, routing, and consumer behavior before enabling actions.

### Action and automation
- ✅ **Use Activator for condition-based actions** — trigger notifications, workflows, functions, or pipeline-style responses from events instead of requiring people to watch dashboards.
- ✅ **Prefer idempotent consumers** — downstream actions should handle duplicate or retried events safely.
- ✅ **Define ownership for each action** — every alert, function, notebook, or automation must have a business owner and runbook.
- ✅ **Start narrow** — pilot one high-value event family before modeling the whole enterprise event catalog.

## Anti-patterns

- 🚫 **Publishing every telemetry row as a business event** — this floods consumers and blurs the difference between signals and meaningful state changes.
- 🚫 **No schema ownership** — inconsistent event contracts create the same integration problems business events are meant to solve.
- 🚫 **Duplicate stream ingestion per team** — Real-Time hub should make reusable streams discoverable; don't reconnect every consumer to the same source.
- 🚫 **Alert storms from broad event filters** — capacity, job, and operational events must be filtered, grouped, or aggregated before human notification.
- 🚫 **Actions without replay or audit thinking** — if an event triggers a business process, store enough context to explain what happened and recover safely.

## References

- [Real-Time hub overview](https://learn.microsoft.com/en-us/fabric/real-time-hub/real-time-hub-overview)
- [Supported sources in Real-Time hub](https://learn.microsoft.com/en-us/fabric/real-time-hub/supported-sources)
- [Get started with Real-Time hub](https://learn.microsoft.com/en-us/fabric/real-time-hub/get-started-real-time-hub)
- [Business events overview](https://learn.microsoft.com/en-us/fabric/real-time-hub/business-events/business-events-overview)
- [Business events concepts](https://learn.microsoft.com/en-us/fabric/real-time-hub/business-events/business-events-concepts)
- [Create business events](https://learn.microsoft.com/en-us/fabric/real-time-hub/business-events/create-business-events)
- [Set alerts on business events](https://learn.microsoft.com/en-us/fabric/real-time-hub/business-events/set-alerts-business-events)
- [Consume business events from Activator](https://learn.microsoft.com/en-us/fabric/real-time-hub/business-events/consume-business-events-from-activator)
- [Mirrored database change feed source](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/add-source-mirrored-database-change-feed)
- [Real-Time Intelligence best practices](real-time-intelligence.md)
