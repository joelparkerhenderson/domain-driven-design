# Event-Driven Architecture in Energy

## Overview

Event-driven architecture (EDA) is a design approach where bounded contexts communicate by producing and consuming domain events rather than making synchronous calls to each other. Events represent significant occurrences within the domain and enable loose coupling, temporal decoupling, and independent scalability of bounded contexts. In the energy domain, EDA is the natural architecture for coordinating the complex, time-sensitive interactions between generation, grid operations, trading, metering, regulatory compliance, and renewable integration.

## Relevance to Energy

Energy systems operate across dramatically different time scales: grid frequency regulation happens in milliseconds, dispatch adjustments in seconds, market clearing in minutes, billing in months, and regulatory compliance over years. Event-driven architecture accommodates these temporal differences by allowing each bounded context to consume events at its own pace. A UnitTripped event is processed immediately by the grid balancing system but may not be reconciled in financial settlement for days.

## Event Flows Between Contexts

### Generation to Grid Operations

When a generating unit changes state (dispatched, ramped, tripped), the Generation Context publishes events that the Transmission & Distribution Context consumes to update power flow calculations and detect potential grid instabilities.

- **UnitDispatched** triggers power flow recalculation at the unit's interconnection point
- **UnitTripped** triggers immediate contingency assessment and potential load shedding evaluation
- **MaintenanceStarted** triggers planned topology adjustments in the grid model

### Generation to Trading

Generation dispatch and availability changes affect market positions and bidding strategies.

- **UnitDispatched** updates actual generation against scheduled generation for settlement reconciliation
- **CapacityChanged** triggers rebidding in upcoming market intervals
- **UnitTripped** triggers real-time market rebalancing and potential emergency energy purchases

### Renewable to Multiple Contexts

Renewable generation events fan out to multiple consumers.

- **GenerationForecastUpdated** flows to Generation (dispatch planning), Trading (market bidding), and Regulatory (reliability assessment)
- **CurtailmentApplied** flows to Trading (position adjustment) and Regulatory (curtailment reporting)
- **RECGenerated** flows to Trading (REC trading) and Regulatory (RPS compliance tracking)
- **DERExporting** flows to T&D (feeder flow monitoring) and Metering & Billing (net metering credit)

### Grid Operations to Billing

Outage events trigger billing adjustments.

- **OutageDetected** starts the outage clock for affected customer accounts
- **ServiceRestored** calculates outage duration and triggers billing credit evaluation
- **SubstationAlarmTriggered** may trigger preventive notifications to affected customers

### Regulatory to All Contexts

Regulatory events propagate constraints and rules to all operational contexts.

- **DemandResponseActivated** flows to Generation (adjust dispatch), Metering & Billing (calculate DR credits), and Renewable (dispatch DER and storage)
- **MarketRuleChanged** flows to Trading (update bidding logic) and Generation (update dispatch constraints)
- **TariffApproved** flows to Metering & Billing (apply new rates)

## Event Infrastructure Patterns

### Event Bus

A central messaging infrastructure that routes events between bounded contexts. In the energy domain, the event bus must support:

- High throughput for meter reading events (millions per day)
- Low latency for grid operation events (milliseconds)
- Guaranteed delivery for financial events (trade executions, settlements)
- Event ordering within a single aggregate stream
- Dead letter queues for events that fail processing

### Event Store

A persistent log of all domain events, serving as both the communication mechanism and an audit trail. Energy-specific requirements include:

- Long retention periods for regulatory compliance (NERC requires 3-6 years for many records)
- Time-series optimized storage for meter readings and generation output
- Efficient replay capability for settlement recalculation and dispute resolution
- Partitioning by bounded context for independent scaling

### Event Sourcing

Some energy aggregates benefit from event sourcing, where the aggregate state is reconstructed from its event history rather than stored as a snapshot:

- **GeneratingUnit**: Reconstructing dispatch history from UnitDispatched events enables analysis and regulatory audit
- **Outage**: Reconstructing outage progression from detection through restoration events provides a complete audit trail
- **MarketPosition**: Reconstructing position from trade execution events enables settlement verification

### Saga Pattern

Long-running processes that span multiple bounded contexts use the saga pattern:

- **Outage-to-Credit Saga**: OutageDetected (T&D) -> track duration -> ServiceRestored (T&D) -> calculate credit -> apply to BillingCycle (Metering & Billing)
- **Dispatch-to-Settlement Saga**: DispatchInstructionIssued (Trading) -> UnitDispatched (Generation) -> meter actual output -> SettlementCalculated (Trading)
- **Curtailment Saga**: LineOverloaded (T&D) -> CurtailmentApplied (Renewable) -> dispatch adjustment (Generation) -> settlement adjustment (Trading)

## Temporal Considerations

- **Real-time events** (milliseconds to seconds): Grid faults, frequency deviations, SCADA alarms
- **Near-real-time events** (seconds to minutes): Unit dispatch, market price updates, meter readings
- **Batch events** (hours to days): Settlement calculations, compliance reports, billing cycles
- **Periodic events** (months to years): Capacity market auctions, regulatory filings, tariff changes

## Relationships to Other Topics

- [Domain Events](../domain-events/) -- Domain events are the building blocks of event-driven architecture
- [Bounded Contexts](../bounded-contexts/) -- EDA connects bounded contexts through events
- [Integration Patterns](../integration-patterns/) -- EDA is a primary integration pattern
- [Context Map](../context-map/) -- Event flows are documented on the context map
- [Aggregates](../aggregates/) -- Aggregates produce and consume domain events

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 11: Domain Events. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Richards, Mark. "Software Architecture Patterns." O'Reilly, 2015.
