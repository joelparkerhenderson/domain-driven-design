# Aggregates in Energy

## Overview

An aggregate is a cluster of domain objects that are treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity (the aggregate root) through which all external access is managed. The aggregate root enforces invariants -- business rules that must always be true -- across all objects within the aggregate boundary. In the energy domain, aggregates model the transactional boundaries of generating units, grid assets, market positions, meter accounts, compliance obligations, and renewable assets.

## Relevance to Energy

Energy data involves complex relationships: a generating unit has operational parameters and fuel sources; a market position spans multiple contracts and trade executions; a meter account contains readings, tariffs, and billing cycles. Aggregates define which objects must be consistent together in a single transaction and which can be eventually consistent across transactions.

## Energy Aggregates

### GeneratingUnit Aggregate

- **Root**: GeneratingUnit (identified by GeneratingUnitId)
- **Internal entities**: FuelSource, MaintenanceSchedule
- **Internal value objects**: Capacity, HeatRate, RampRate, OperatingStatus
- **Invariants**:
  - A generating unit cannot be dispatched above its rated capacity
  - Heat rate must be positive for thermal units
  - A unit in maintenance status cannot accept dispatch orders
  - Ramp rate limits the maximum change in output per time interval
  - Fuel source must be specified for thermal generating units
- **Lifecycle states**: Available, Dispatched, Ramping, AtCapacity, Maintenance, ForcedOutage, Decommissioned
- **Events produced**: UnitDispatched, UnitRamped, UnitTripped, MaintenanceStarted, MaintenanceCompleted

### TransmissionLine Aggregate

- **Root**: TransmissionLine (identified by TransmissionLineId)
- **Internal value objects**: VoltageRating, ThermalLimit, LineImpedance, Endpoints
- **Invariants**:
  - Power flow cannot exceed the thermal limit of the line
  - Voltage rating is immutable once the line is commissioned
  - A line cannot be in service and under maintenance simultaneously
- **Lifecycle states**: InService, UnderMaintenance, Faulted, Deenergized, Decommissioned
- **Events produced**: LineEnergized, LineDeenergized, LineFaulted, LineRestored

### EnergyContract Aggregate

- **Root**: EnergyContract (identified by ContractId)
- **Internal entities**: ContractTerm, DeliverySchedule
- **Internal value objects**: ContractPrice, ContractVolume, DeliveryPoint, ContractPeriod
- **Invariants**:
  - Contract volume and price must be agreed upon before execution
  - Delivery schedule must fall within the contract period
  - Counterparty credit exposure cannot exceed approved limits
  - A terminated contract cannot accept new delivery schedules
- **Lifecycle states**: Draft, Negotiating, Executed, Active, Expired, Terminated
- **Events produced**: ContractExecuted, ContractAmended, DeliveryScheduled, ContractExpired

### Meter Aggregate

- **Root**: Meter (identified by MeterId)
- **Internal entities**: MeterChannel
- **Internal value objects**: MeterReading, MeterLocation, CommunicationStatus
- **Invariants**:
  - Meter readings must be chronologically ordered within each channel
  - A meter must have at least one active channel
  - Cumulative readings cannot decrease (for non-bidirectional meters)
  - Communication status must be updated within the configured heartbeat interval
- **Lifecycle states**: Installed, Active, Communicating, Offline, Decommissioned
- **Events produced**: MeterReadingReceived, MeterCommunicationLost, MeterCommunicationRestored

### RenewableAsset Aggregate

- **Root**: RenewableAsset (identified by RenewableAssetId)
- **Internal value objects**: RatedCapacity, AssetType, Coordinates, InterconnectionPoint
- **Invariants**:
  - Output cannot exceed rated capacity
  - Curtailment cannot be applied to an offline asset
  - Asset type (solar, wind, hydro) is immutable once registered
  - Interconnection point must be a valid grid node
- **Lifecycle states**: Registered, Commissioning, Operational, Curtailed, Offline, Decommissioned
- **Events produced**: AssetCommissioned, GenerationRecorded, CurtailmentApplied, CurtailmentLifted

### BatteryStorageSystem Aggregate

- **Root**: BatteryStorageSystem (identified by BatteryId)
- **Internal value objects**: StateOfCharge, MaxCapacity, ChargeRate, DischargeRate, CycleCount
- **Invariants**:
  - State of charge must be between 0% and 100%
  - Charge rate cannot exceed the rated maximum charge rate
  - Discharge rate cannot exceed the rated maximum discharge rate
  - System cannot simultaneously charge and discharge
  - Cycle count increments with each full charge-discharge cycle
- **Lifecycle states**: Idle, Charging, Discharging, Standby, Maintenance, Offline
- **Events produced**: ChargingStarted, DischargingStarted, SOCThresholdReached, CycleCompleted

## Aggregate Design Rules

1. **Keep aggregates small**: Prefer smaller aggregates with fewer internal objects. A GeneratingUnit contains its operational parameters but references a PowerPlant by PowerPlantId, not by containing the PowerPlant object.

2. **Reference other aggregates by identity**: A Meter references a CustomerAccount by CustomerAccountId. An EnergyContract references counterparties by CounterpartyId.

3. **Use domain events for cross-aggregate coordination**: When a UnitDispatched event occurs, the Trading context updates market positions -- but this happens asynchronously, not in the same transaction.

4. **Design for eventual consistency between aggregates**: Accept that the Billing context may learn about a new meter reading milliseconds after it is recorded, not at the exact same instant.

5. **One transaction per aggregate**: Never modify multiple aggregates in a single database transaction. If you need to update both a GeneratingUnit and a MarketPosition, use domain events to coordinate.

## Relationships to Other Topics

- [Entities](../entities/) -- Aggregate roots are entities; aggregates contain entities
- [Value Objects](../value-objects/) -- Aggregates contain value objects for immutable domain concepts
- [Domain Events](../domain-events/) -- Aggregates produce events to communicate changes
- [Repositories](../repositories/) -- One repository per aggregate root
- [Domain Services](../domain-services/) -- Coordinate operations across multiple aggregates
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context defines its own aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10: Aggregates. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 10: Aggregates. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 5: Tactical Design with Aggregates. Addison-Wesley, 2016.
