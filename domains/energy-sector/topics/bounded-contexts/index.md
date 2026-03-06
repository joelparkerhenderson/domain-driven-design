# Bounded Contexts in Energy

## Overview

A bounded context is a logical boundary within which a particular domain model is defined and applicable. Each bounded context has its own ubiquitous language, its own set of aggregates, entities, value objects, and domain events, and is owned by a specific team. In the energy domain, bounded contexts align with the distinct operational areas of the energy lifecycle: generation, transmission and distribution, trading and markets, metering and billing, regulatory compliance, and renewable integration.

## Relevance to Energy

Energy systems involve fundamentally different operational concerns that use the same terms with different meanings. The word "capacity" means maximum generation output in the Generation Context, available transfer capability in the Transmission Context, market obligation in the Regulatory Context, and battery charge capacity in the Renewable Integration Context. Bounded contexts ensure that each meaning is unambiguous within its boundary.

## Energy Bounded Contexts

### Generation Context

Responsible for power plant operations including unit dispatch, capacity planning, fuel management, and maintenance scheduling. The core concept is the GeneratingUnit, which represents a single dispatchable power source.

- Key aggregates: GeneratingUnit, PowerPlant, FuelContract
- Team: Generation operations engineers and plant dispatchers
- Temporal focus: Real-time dispatch (seconds to minutes) and long-term capacity planning (years)

### Transmission & Distribution Context

Responsible for the physical delivery of electricity from generators to consumers through the high-voltage transmission network and medium-voltage distribution system. The core concepts are TransmissionLine, Substation, and Outage.

- Key aggregates: TransmissionLine, Substation, Outage, DistributionFeeder
- Team: Grid operators, transmission planners, and field crews
- Temporal focus: Real-time monitoring (milliseconds) and planned maintenance (weeks)

### Trading & Market Context

Responsible for wholesale energy market participation, contract management, and financial settlement. The core concepts are EnergyContract, MarketBid, and Settlement.

- Key aggregates: EnergyContract, MarketPosition, TradeExecution, Settlement
- Team: Energy traders, risk analysts, and settlement specialists
- Temporal focus: Day-ahead scheduling (24 hours) and real-time pricing (5 minutes)

### Metering & Billing Context

Responsible for measuring electricity consumption and generation at customer premises and calculating charges. The core concepts are Meter, MeterReading, and BillingCycle.

- Key aggregates: Meter, CustomerAccount, BillingCycle, Invoice
- Team: Metering engineers, billing analysts, and customer service representatives
- Temporal focus: Interval data (15 minutes) and billing periods (monthly)

### Regulatory & Grid Context

Responsible for ensuring compliance with FERC, NERC, and state regulatory requirements, administering demand response programs, and providing ancillary services. The core concepts are ComplianceObligation, DemandResponseProgram, and AncillaryServiceCommitment.

- Key aggregates: ComplianceObligation, DemandResponseProgram, CapacityCommitment, AncillaryServiceOffer
- Team: Regulatory compliance officers, reliability engineers, and demand response coordinators
- Temporal focus: Compliance reporting (quarterly/annual) and demand response events (hours)

### Renewable Integration Context

Responsible for managing variable renewable generation assets, battery storage systems, and distributed energy resources. The core concepts are RenewableAsset, BatteryStorageSystem, and GenerationForecast.

- Key aggregates: RenewableAsset, BatteryStorageSystem, DERPortfolio, GenerationForecast
- Team: Renewable asset managers, storage operators, and forecasting analysts
- Temporal focus: Forecast horizons (hours to days) and storage dispatch (minutes)

## Context Boundaries and Ownership

Each bounded context:

- Owns its domain model and does not share database tables with other contexts
- Publishes domain events to communicate state changes to other contexts
- Defines its own aggregate roots, entities, and value objects
- Has a dedicated team responsible for its evolution
- Communicates with other contexts through well-defined interfaces

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Bounded contexts are the primary output of strategic design
- [Context Map](../context-map/) -- Documents relationships between bounded contexts
- [Ubiquitous Language](../ubiquitous-language/) -- Each bounded context defines its own language
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects context boundaries from external concepts
- [Subdomains](../subdomains/) -- Bounded contexts often align with subdomain classifications

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7: Modeling Bounded Contexts. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
