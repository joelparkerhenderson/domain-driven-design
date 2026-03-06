# Entities in Energy

## Overview

An entity is a domain object that has a unique identity which persists over time, even as its attributes change. Entities are distinguished by their identity rather than their attributes -- two entities with the same attributes but different identities are considered different objects. In the energy domain, entities represent the persistent, identifiable objects that energy systems track, manage, and operate throughout their lifecycle.

## Relevance to Energy

Energy systems manage long-lived assets and relationships that must be tracked over years or decades. A generating unit installed in 2005 is the same unit in 2025, even though its heat rate, capacity rating, and maintenance history have changed. A customer account persists through address changes, tariff updates, and meter replacements. Entities provide the identity continuity that energy operations require.

## Energy Entities

### Generation Context Entities

- **GeneratingUnit**: Identified by GeneratingUnitId. Represents a single power-producing machine. Attributes change over time (operating status, cumulative runtime, current output) but identity persists through the unit's entire operational life. A generating unit may be retrofitted, derated, or repowered, but it retains its identity.
- **PowerPlant**: Identified by PowerPlantId. A facility containing one or more generating units. The plant persists even as individual units are added, retired, or replaced.
- **FuelContract**: Identified by FuelContractId. An agreement for fuel supply to a generating station. The contract's terms, delivery schedules, and pricing may be amended, but its identity persists.

### Transmission & Distribution Context Entities

- **Substation**: Identified by SubstationId. A facility that transforms voltage and routes power. Equipment within the substation changes over time, but the substation retains its identity and geographic location.
- **TransmissionLine**: Identified by TransmissionLineId. A physical conductor connecting grid nodes. Its operating parameters may change with upgrades, but the line retains its identity.
- **Outage**: Identified by OutageId. An event representing a service interruption. The outage progresses through states (detected, diagnosed, crew dispatched, restored) while retaining its identity.
- **DistributionFeeder**: Identified by FeederId. A medium-voltage circuit serving end-use customers. Configuration changes over time but the feeder retains its identity.

### Trading & Market Context Entities

- **EnergyContract**: Identified by ContractId. A financial or physical agreement that progresses through negotiation, execution, delivery, and expiration.
- **TradeExecution**: Identified by TradeExecutionId. A specific market trade that records the moment of execution and its terms. Price, volume, and counterparty are fixed at execution.
- **MarketPosition**: Identified by PositionId. The net exposure of the trading portfolio at a given delivery point and time period. The position changes with each new trade but persists as an ongoing concern.

### Metering & Billing Context Entities

- **Meter**: Identified by MeterId. A physical or virtual metering device that measures electricity flow. The meter persists through firmware upgrades, communication module replacements, and recalibration.
- **CustomerAccount**: Identified by AccountId. A customer's relationship with the utility, persisting through address changes, tariff updates, and service modifications.
- **Invoice**: Identified by InvoiceId. A billing document generated for a specific billing cycle. The invoice may be adjusted or credited but retains its identity.

### Regulatory & Grid Context Entities

- **ComplianceObligation**: Identified by ObligationId. A specific regulatory requirement that must be met, tracked from identification through evidence collection to reporting.
- **DemandResponseEvent**: Identified by EventId. A specific demand response activation that progresses from notification through performance measurement to settlement.

### Renewable Integration Context Entities

- **RenewableAsset**: Identified by RenewableAssetId. A solar array, wind turbine, or other renewable generation facility. The asset persists through equipment upgrades and repowering.
- **BatteryStorageSystem**: Identified by BatteryId. A battery installation that persists through charge-discharge cycles, degradation, and module replacements.
- **DERRegistration**: Identified by RegistrationId. A distributed energy resource's enrollment in utility programs, persisting through equipment changes.

## Entity Design Guidelines

- **Identity is immutable**: Once assigned, an entity's identity never changes. A GeneratingUnit's GeneratingUnitId is permanent.
- **Identity is the primary equality test**: Two GeneratingUnit objects with the same GeneratingUnitId are the same unit, regardless of attribute differences.
- **Entities have lifecycle**: Entities progress through states (e.g., a GeneratingUnit progresses from Commissioning to Operational to Decommissioned).
- **Entities are mutable**: Unlike value objects, entity attributes change over time. A Meter's communication status, a Substation's equipment configuration, and a Contract's delivery schedule all change.
- **Minimize entity attributes**: Include only the attributes necessary for the entity's role within its bounded context. A GeneratingUnit in the Trading context needs fewer attributes than the same unit in the Generation context.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Entities serve as aggregate roots or exist within aggregates
- [Value Objects](../value-objects/) -- Entities contain value objects for immutable descriptive data
- [Repositories](../repositories/) -- Repositories persist and retrieve entities by identity
- [Domain Events](../domain-events/) -- Entities produce events when significant state changes occur
- [Ubiquitous Language](../ubiquitous-language/) -- Entity names reflect the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5: Entities. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 8: Architectural Patterns. O'Reilly, 2021.
