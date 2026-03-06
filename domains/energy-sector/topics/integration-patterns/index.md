# Integration Patterns in Energy

## Overview

Integration patterns define how bounded contexts communicate and share data while maintaining their autonomy and model integrity. Domain-Driven Design identifies several integration patterns -- Shared Kernel, Published Language, Open Host Service, Customer-Supplier, Conformist, and Anti-Corruption Layer -- each appropriate for different relationship dynamics. In the energy domain, these patterns govern how generation, grid operations, trading, metering, regulatory, and renewable contexts exchange information.

## Relevance to Energy

Energy systems must integrate with numerous internal contexts and external systems: ISO/RTO market platforms, SCADA systems, advanced metering infrastructure, weather services, regulatory reporting systems, and counterparty trading platforms. Each integration boundary requires a deliberate pattern choice based on the relationship dynamics, data volumes, latency requirements, and organizational authority.

## Integration Patterns Applied to Energy

### Shared Kernel

A shared kernel is a small, explicitly shared model that two contexts agree to maintain together. Changes to the kernel require coordination between both teams.

- **Dispatch Schedule Format**: The Generation Context and Trading & Market Context share a common dispatch schedule model. Both teams agree on the structure of dispatch intervals, unit assignments, and output levels. Changes to this format require joint review.
- **Generation Forecast Format**: The Renewable Integration Context and Generation Context share a common forecast model including time horizons, granularity, confidence intervals, and unit identification.
- **Grid Node Reference**: Multiple contexts share a common grid node identifier scheme to ensure consistent locational references across generation, transmission, and trading.

### Published Language

A published language is a well-documented, versioned data format that an upstream context publishes for downstream consumers. The upstream context owns the format; downstream contexts adapt to it.

- **Market Clearing Results**: The Trading & Market Context publishes market clearing results in a documented format that the Generation Context consumes for dispatch and the Regulatory Context consumes for compliance monitoring.
- **Outage Reports**: The Transmission & Distribution Context publishes outage reports in a standardized format consumed by Metering & Billing (for billing credits) and Regulatory (for reliability reporting).
- **Meter Data**: The Metering & Billing Context publishes validated meter data in a standard interval data format (aligned with Green Button or NAESB standards) for consumption by Trading (large customer settlement) and Regulatory (demand response verification).

### Open Host Service

An open host service exposes a bounded context's capabilities through a well-defined API that multiple consumers can use without custom integration for each consumer.

- **Generation Status API**: The Generation Context exposes real-time unit status, output levels, and available capacity through an API consumed by Trading (market bidding), T&D (power flow), and Regulatory (reliability monitoring).
- **Grid Topology API**: The Transmission & Distribution Context exposes the current grid topology and equipment status through an API consumed by Generation (dispatch feasibility), Trading (congestion assessment), and Regulatory (contingency analysis).
- **Meter Data API**: The Metering & Billing Context exposes validated meter data through an API consumed by multiple downstream contexts and external reporting systems.

### Customer-Supplier

In a customer-supplier relationship, the downstream context (customer) negotiates with the upstream context (supplier) to get the data it needs.

- **T&D as Supplier to Metering & Billing**: The Metering & Billing Context (customer) needs outage duration and affected customer data from the T&D Context (supplier). The billing team negotiates the data format and delivery timing with the grid operations team.
- **Renewable as Supplier to Metering & Billing**: The Metering & Billing Context (customer) needs DER generation data from the Renewable Integration Context (supplier) for net metering calculations. The billing team specifies the data granularity and delivery frequency.

### Conformist

In a conformist relationship, the downstream context accepts the upstream context's model without negotiation, typically because the upstream context has organizational authority.

- **Regulatory as Authority**: The Regulatory & Grid Context publishes tariff structures, market rules, and compliance requirements that other contexts must conform to. The Generation, Trading, and Metering contexts accept these rules as given, without negotiating the format or content.
- **ISO/RTO as Authority**: External ISO/RTO platforms define market rules, bid formats, and settlement schemas. The Trading & Market Context conforms to these external models (mediated through an ACL).

### Anti-Corruption Layer

An ACL translates between an external model and the internal domain model, preventing external concepts from corrupting the bounded context. See the [Anti-Corruption Layer](../anti-corruption-layer/) topic for detailed energy-specific ACL examples.

## External System Integration

### SCADA Integration

SCADA systems provide real-time telemetry from substations and generating units. Integration considerations:

- High-frequency data (sub-second updates) requires streaming integration
- SCADA uses point-based addressing; the ACL maps SCADA points to domain concepts
- Protocol translation: DNP3, IEC 61850, Modbus to internal domain events
- Data quality flags must be translated to domain-meaningful quality indicators

### ISO/RTO Market Integration

ISO/RTO market platforms require bidirectional integration for market participation:

- Bid submission via ISO-specific APIs and file formats
- Market clearing result retrieval
- Settlement data exchange
- Each ISO (PJM, ERCOT, CAISO, MISO, ISO-NE, NYISO, SPP) has distinct interfaces

### Weather Service Integration

Weather data providers supply forecasting inputs for renewable generation:

- Multiple providers with different data formats, spatial resolutions, and update frequencies
- The ACL normalizes provider-specific formats into internal WeatherForecast value objects
- Forecast ensemble techniques may combine data from multiple providers

### AMI Integration

Advanced metering infrastructure delivers meter readings from the smart meter fleet:

- Millions of interval readings per day require batch-oriented integration
- Head-end systems vary by meter manufacturer (Itron, Landis+Gyr, Sensus)
- The ACL translates vendor-specific data formats to internal MeterReading value objects

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs are a specific integration pattern
- [Context Map](../context-map/) -- Integration patterns are documented on the context map
- [Event-Driven Architecture](../event-driven-architecture/) -- Events are a primary integration mechanism
- [Bounded Contexts](../bounded-contexts/) -- Integration patterns connect bounded contexts
- [Energy Standards](../energy-standards/) -- Standards define many integration formats

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps and Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Integration Patterns. O'Reilly, 2021.
