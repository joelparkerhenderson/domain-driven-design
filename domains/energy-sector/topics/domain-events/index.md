# Domain Events in Energy

## Overview

A domain event represents a significant occurrence within the domain that other parts of the system need to know about. Domain events are named in past tense (e.g., UnitDispatched, MeterReadingReceived) because they describe something that has already happened. They are immutable records of facts that enable loose coupling between bounded contexts. In the energy domain, domain events coordinate the complex interactions between generation, grid operations, trading, metering, regulatory compliance, and renewable integration.

## Relevance to Energy

Energy systems operate in real time with strict reliability requirements. A generating unit tripping offline must immediately trigger rebalancing in the grid, repricing in the market, and potential demand response activation. Domain events enable these cross-context reactions without creating tight coupling between the generation, transmission, trading, and regulatory contexts. Each context reacts to events according to its own rules and at its own pace.

## Generation Context Events

- **UnitDispatched**: A generating unit has been directed to produce a specific output. Carries: GeneratingUnitId, DispatchedMW, DispatchInterval, DispatchReason.
- **UnitRamped**: A generating unit has changed its output level. Carries: GeneratingUnitId, PreviousMW, CurrentMW, RampDuration.
- **UnitTripped**: A generating unit has unexpectedly gone offline. Carries: GeneratingUnitId, TripTime, TripCause, LostCapacityMW.
- **MaintenanceStarted**: A generating unit has entered a planned maintenance period. Carries: GeneratingUnitId, MaintenanceWindowStart, ExpectedDuration.
- **MaintenanceCompleted**: A generating unit has returned to service after maintenance. Carries: GeneratingUnitId, ActualDuration, ReturnToServiceTime.
- **FuelDeliveryReceived**: A fuel shipment has been received at a generation station. Carries: PowerPlantId, FuelType, Quantity, DeliveryDate.

## Transmission & Distribution Context Events

- **OutageDetected**: An interruption in service has been identified. Carries: OutageId, AffectedArea, DetectionTime, EstimatedCustomersAffected.
- **FaultIsolated**: A fault on the grid has been isolated by protective equipment. Carries: FaultId, AffectedEquipment, IsolationTime.
- **ServiceRestored**: Power has been restored to an affected area. Carries: OutageId, RestorationTime, RestoredCustomers.
- **LineOverloaded**: A transmission line is approaching or exceeding its thermal limit. Carries: TransmissionLineId, CurrentFlow, ThermalLimit, OverloadPercentage.
- **SubstationAlarmTriggered**: A substation monitoring system has detected an abnormal condition. Carries: SubstationId, AlarmType, AlarmTime, Severity.

## Trading & Market Context Events

- **TradeExecuted**: A market trade has been completed. Carries: TradeExecutionId, ContractId, Volume, Price, DeliveryPoint, Counterparty.
- **MarketPricePublished**: A new locational marginal price has been published. Carries: NodeId, LMP, EnergyComponent, CongestionComponent, LossComponent, Timestamp.
- **SettlementCalculated**: Financial settlement for a period has been calculated. Carries: SettlementId, SettlementPeriod, NetAmount, MarketParticipantId.
- **ContractExecuted**: A new energy contract has been finalized. Carries: ContractId, ContractType, Volume, Price, ContractPeriod, CounterpartyId.
- **PositionLimitBreached**: A trading position has exceeded risk limits. Carries: PositionId, CurrentExposure, ApprovedLimit, BreachTime.

## Metering & Billing Context Events

- **MeterReadingReceived**: A consumption or generation measurement has been recorded. Carries: MeterId, ReadingValue, ReadingTimestamp, Channel, ReadingType.
- **MeterCommunicationLost**: A smart meter has stopped communicating. Carries: MeterId, LastCommunicationTime, ExpectedInterval.
- **BillingCycleCompleted**: A billing cycle has been calculated and an invoice generated. Carries: AccountId, BillingPeriod, TotalCharges, InvoiceId.
- **TariffUpdated**: A tariff rate schedule has been modified. Carries: TariffId, EffectiveDate, PreviousRates, NewRates.
- **NetMeteringCreditApplied**: A DER owner has been credited for exported electricity. Carries: AccountId, MeterId, ExportedMWh, CreditAmount, Period.

## Regulatory & Grid Context Events

- **DemandResponseActivated**: A demand response event has been triggered. Carries: ProgramId, ActivationTime, TargetReductionMW, Duration, AffectedZone.
- **ComplianceViolationDetected**: A regulatory compliance standard has been breached. Carries: ObligationId, Standard, ViolationType, DetectionTime, Severity.
- **AncillaryServiceDeployed**: An ancillary service (frequency regulation, spinning reserve) has been called upon. Carries: ServiceType, DeploymentTime, RequestedMW, Duration.
- **CapacityCommitmentRecorded**: A capacity market obligation has been recorded. Carries: CommitmentId, CapacityMW, DeliveryPeriod, PricePerkWMonth.

## Renewable Integration Context Events

- **GenerationForecastUpdated**: A renewable generation forecast has been revised. Carries: AssetId, ForecastHorizon, ForecastedMWh, ConfidenceInterval, UpdateReason.
- **CurtailmentApplied**: Renewable generation has been intentionally reduced. Carries: AssetId, CurtailedMW, CurtailmentReason, StartTime, ExpectedDuration.
- **CurtailmentLifted**: A curtailment order has been removed. Carries: AssetId, RestoredMW, LiftTime.
- **BatteryChargingStarted**: A battery storage system has begun charging. Carries: BatteryId, ChargeRateMW, TargetSOC, StartTime.
- **BatteryDischargingStarted**: A battery storage system has begun discharging. Carries: BatteryId, DischargeRateMW, CurrentSOC, StartTime.
- **RECGenerated**: A renewable energy certificate has been created. Carries: RECId, AssetId, MWhGenerated, VintageMonth, TechnologyType.

## Event Design Principles

- **Past tense naming**: Events describe what has already happened (UnitDispatched, not DispatchUnit)
- **Immutability**: Events are facts that cannot be changed after they are published
- **Self-contained**: Events carry enough data for consumers to react without querying the source context
- **Ordered within aggregate**: Events from a single aggregate are ordered; events across aggregates are not guaranteed to be ordered
- **Idempotent consumers**: Event handlers must handle duplicate delivery gracefully

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Aggregates produce domain events when state changes
- [Event-Driven Architecture](../event-driven-architecture/) -- Domain events are the foundation of event-driven systems
- [Bounded Contexts](../bounded-contexts/) -- Events enable loose coupling between contexts
- [Integration Patterns](../integration-patterns/) -- Events are a primary integration mechanism
- [Repositories](../repositories/) -- Event sourcing stores aggregates as event streams

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 11: Domain Events. O'Reilly, 2021.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
