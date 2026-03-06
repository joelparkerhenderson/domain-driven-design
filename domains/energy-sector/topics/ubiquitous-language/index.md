# Ubiquitous Language in Energy

## Overview

Ubiquitous language is the shared vocabulary that domain experts, developers, and stakeholders use to communicate about the domain. It is embedded in the code, documentation, and conversations. Each bounded context defines its own ubiquitous language where terms have precise, unambiguous meanings. In the energy domain, ubiquitous language bridges the gap between grid operators, power traders, utility engineers, regulatory analysts, and software developers.

## Relevance to Energy

Energy is a terminology-rich domain where the same word carries different meanings depending on context. "Capacity" means maximum generation output to a plant engineer, available transfer capability to a grid operator, a market product to a trader, and a regulatory obligation to a compliance officer. Ubiquitous language ensures that within each bounded context, every term has one clear definition that all team members -- technical and non-technical -- share.

## Language by Bounded Context

### Generation Context Language

- **Generating Unit**: A single power-producing machine or facility identified by a unique unit ID
- **Dispatch**: Directing a generating unit to produce a specific power output at a given time
- **Heat Rate**: Fuel efficiency of a thermal unit, measured in BTU per kilowatt-hour
- **Capacity Factor**: Ratio of actual output to maximum possible output over a period
- **Ramp Rate**: The speed at which a generating unit can increase or decrease its power output
- **Forced Outage**: An unplanned removal of a generating unit from service due to equipment failure
- **Maintenance Window**: A scheduled period during which a generating unit is taken offline for planned maintenance

### Transmission & Distribution Context Language

- **Transmission Line**: A high-voltage conductor carrying electricity across the grid
- **Substation**: A facility that transforms voltage levels and routes power flow
- **Fault**: An abnormal electrical condition such as a short circuit
- **Outage**: An interruption in electricity supply, planned or unplanned
- **Power Flow**: The pattern of electricity movement through the transmission network
- **Contingency**: A potential equipment failure scenario used in reliability planning

### Trading & Market Context Language

- **Locational Marginal Price (LMP)**: The price of electricity at a specific grid node
- **Day-Ahead Market**: A wholesale market where electricity is traded for next-day delivery
- **Real-Time Market**: A wholesale market that balances supply and demand in 5-minute intervals
- **Energy Contract**: An agreement to buy or sell energy at defined terms
- **Settlement**: Financial reconciliation of actual vs. scheduled energy transactions
- **Position**: A trader's net exposure to market price movements

### Metering & Billing Context Language

- **Meter Reading**: A recorded measurement of consumption or generation at a specific interval
- **Tariff**: A published schedule of rates and charges for electricity service
- **Billing Cycle**: The recurring period over which consumption is measured and billed
- **Net Metering**: A billing mechanism crediting DER owners for exported electricity
- **Demand Charge**: A charge based on the customer's peak power draw during a billing period

### Regulatory & Grid Context Language

- **Demand Response**: Programs incentivizing consumers to reduce or shift consumption during peak periods
- **Ancillary Services**: Support services maintaining grid reliability (frequency regulation, reserves)
- **Balancing Authority**: The entity responsible for balancing supply and demand in a region
- **Compliance Obligation**: A specific regulatory requirement that must be met and documented
- **Capacity Market**: A market mechanism ensuring sufficient generation capacity for future demand

### Renewable Integration Context Language

- **Curtailment**: Intentional reduction of renewable generation output when supply exceeds demand
- **Distributed Energy Resource (DER)**: Small-scale generation or storage at or near consumption point
- **State of Charge (SOC)**: Current energy level of a battery as a percentage of capacity
- **Generation Forecast**: A prediction of expected renewable output based on weather and historical data
- **Renewable Energy Certificate (REC)**: Proof that one MWh was generated from a renewable source
- **Intermittency**: The variable and non-dispatchable nature of wind and solar generation

## Language Governance

- Maintain a living glossary accessible to all team members
- Review and update language definitions during each sprint or iteration
- Flag and resolve ambiguities when the same term appears in multiple contexts
- Embed the ubiquitous language in code: class names, method names, and variable names should use domain terms
- Use the ubiquitous language in all documentation, tests, and stakeholder communications

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Ubiquitous language is a core strategic design practice
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context defines its own language
- [Context Map](../context-map/) -- Language translation occurs at context boundaries
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate between external and internal language
- [Entities](../entities/) -- Entity names reflect the ubiquitous language
- [Value Objects](../value-objects/) -- Value object names use domain terminology

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 2: Communication and the Use of Language. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 1: Getting Started with DDD. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 2: Discovering Domain Knowledge. O'Reilly, 2021.
- Evans, Eric. "Domain Language" (website). https://www.domainlanguage.com/
