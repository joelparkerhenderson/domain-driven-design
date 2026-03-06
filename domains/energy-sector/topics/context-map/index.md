# Context Map in Energy

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a system. It documents how contexts communicate, which context is upstream or downstream, and what integration patterns are used at each boundary. In the energy domain, the context map captures the complex interdependencies between generation, grid operations, market participation, customer billing, regulatory compliance, and renewable integration.

## Relevance to Energy

Energy systems have intricate dependencies between operational areas. Generation dispatch decisions depend on market prices and grid constraints. Billing depends on meter data and tariff structures set by regulators. Renewable curtailment decisions require coordination between generation forecasting, grid capacity, and market conditions. The context map makes these relationships explicit and manageable.

## Context Relationships

### Generation Context Relationships

- **Generation --> Trading & Market Context** (Upstream-Downstream): Generation publishes available capacity and dispatch status; Trading uses this to inform market bidding strategies. Integration pattern: Published Language via dispatch schedules and capacity offers.
- **Generation --> Transmission & Distribution Context** (Upstream-Downstream): Generation publishes unit output levels; T&D uses these for power flow analysis. Integration pattern: Open Host Service exposing real-time generation data.
- **Generation <-- Regulatory & Grid Context** (Conformist): Generation conforms to dispatch orders and reliability requirements issued by the Regulatory & Grid Context.

### Transmission & Distribution Context Relationships

- **T&D --> Metering & Billing Context** (Upstream-Downstream): T&D publishes outage events and service restoration times; Metering & Billing uses these for billing adjustments and outage credits. Integration pattern: Domain events.
- **T&D --> Regulatory & Grid Context** (Upstream-Downstream): T&D publishes grid reliability metrics and outage reports; Regulatory uses these for compliance reporting. Integration pattern: Published Language.
- **T&D <-- Generation Context** (Customer-Supplier): T&D receives generation output data to manage power flow and grid stability.

### Trading & Market Context Relationships

- **Trading --> Generation Context** (Upstream-Downstream): Trading issues dispatch instructions based on market clearing; Generation executes dispatch orders. Integration pattern: Shared Kernel for dispatch schedule format.
- **Trading <-- Regulatory & Grid Context** (Conformist): Trading conforms to market rules, position limits, and settlement procedures defined by the Regulatory context.
- **Trading --> Metering & Billing Context** (Upstream-Downstream): Trading publishes wholesale price data; Billing uses market prices for large commercial and industrial customer settlement.

### Metering & Billing Context Relationships

- **Metering & Billing <-- Regulatory & Grid Context** (Conformist): Billing conforms to tariff structures, rate schedules, and net metering rules set by regulators.
- **Metering & Billing <-- Renewable Integration Context** (Customer-Supplier): Billing receives DER generation data for net metering credit calculations.

### Regulatory & Grid Context Relationships

- **Regulatory --> All Contexts** (Upstream): Regulatory publishes compliance requirements, tariff structures, and reliability standards that all other contexts must respect.
- **Regulatory <-- External ISO/RTO** (Anti-Corruption Layer): The Regulatory context maintains an ACL to translate ISO/RTO market rules, reliability standards, and compliance reporting formats into the internal domain model.

### Renewable Integration Context Relationships

- **Renewable --> Generation Context** (Partnership): Renewable generation forecasts are shared with the Generation context for integrated dispatch planning. Integration pattern: Shared Kernel for generation forecast format.
- **Renewable --> Trading & Market Context** (Upstream-Downstream): Renewable publishes generation forecasts and REC availability; Trading uses these for market bidding and REC trading.
- **Renewable --> Metering & Billing Context** (Upstream-Downstream): Renewable publishes DER generation data for net metering calculations.

## Integration Patterns Used

- **Published Language**: Standardized data formats for dispatch schedules, market bids, and compliance reports
- **Open Host Service**: Real-time APIs for generation output, grid status, and meter data
- **Anti-Corruption Layer**: Translation boundaries with ISO/RTO systems, SCADA, and legacy billing platforms
- **Shared Kernel**: Common formats shared between Generation and Trading for dispatch, and between Generation and Renewable for forecasting
- **Conformist**: Contexts that must comply with regulatory mandates without negotiation
- **Customer-Supplier**: Negotiated interfaces between operational contexts

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- The context map documents relationships between bounded contexts
- [Strategic Design](../strategic-design/) -- Context mapping is a strategic design activity
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs appear as integration patterns on the context map
- [Integration Patterns](../integration-patterns/) -- Detailed documentation of integration approaches
- [Domain Events](../domain-events/) -- Many context relationships are mediated by domain events

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
