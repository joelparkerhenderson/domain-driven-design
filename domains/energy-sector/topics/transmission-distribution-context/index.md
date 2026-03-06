# Transmission & Distribution Context in Energy

## Overview

The Transmission & Distribution Context is the bounded context responsible for managing the physical delivery of electricity from generation sources to end-use consumers. It encompasses the high-voltage transmission network, substations, medium-voltage distribution system, and the real-time monitoring and control of power flow across the grid. This context owns outage detection, fault isolation, service restoration, and grid topology management.

## Relevance to Energy

The transmission and distribution network is the physical backbone of the energy system. Every kilowatt-hour generated must traverse this network to reach consumers. The T&D Context manages the most time-critical operations in the energy domain: grid disturbances must be detected in milliseconds, faults must be isolated in seconds, and restoration must be coordinated within minutes to hours. It translates the abstract concepts of power flow and grid reliability into concrete operational actions.

## Bounded Context Boundaries

**Owns**: TransmissionLine aggregate, Substation aggregate, Outage aggregate, DistributionFeeder aggregate, grid topology model, power flow calculations, outage management

**Does not own**: Generation dispatch (Generation Context), market pricing (Trading & Market Context), customer billing adjustments (Metering & Billing Context), reliability standards (Regulatory & Grid Context)

**Upstream contexts**: Generation Context (generation output levels), Renewable Integration Context (DER output and forecasts)

**Downstream contexts**: Metering & Billing Context (outage duration for billing credits), Regulatory & Grid Context (reliability metrics and outage reports)

## Core Capabilities

### Grid Topology Management

Grid topology management maintains an accurate, real-time model of the physical electrical network.

- Maintain a model of all transmission lines, substations, transformers, switches, and distribution feeders
- Track equipment status (in-service, de-energized, faulted, under maintenance)
- Update topology in real time as switching operations change the network configuration
- Support connectivity analysis to determine which customers are served by which feeders and substations
- Maintain voltage level hierarchy from high-voltage transmission through medium-voltage distribution

### Power Flow Monitoring

Power flow monitoring tracks the movement of electricity through the network in real time.

- Receive real-time telemetry from SCADA systems at substations and transmission lines
- Calculate power flow on each transmission line and transformer
- Monitor voltage levels at each bus and substation
- Detect thermal overloads when line flow approaches or exceeds rated limits
- Identify voltage violations when bus voltage deviates from acceptable ranges
- Monitor power factor and reactive power flow for voltage stability

### Outage Management

Outage management coordinates the detection, diagnosis, and restoration of service interruptions.

- Detect outages through SCADA alarms, smart meter last-gasp signals, and customer reports
- Correlate multiple indicators to determine the scope and location of the outage
- Dispatch field crews to the outage location with appropriate equipment and safety procedures
- Coordinate switching operations to isolate the faulted section and restore service to unaffected areas
- Track outage progress from detection through diagnosis, crew dispatch, repair, and restoration
- Calculate outage duration and customer-minutes-interrupted for reliability reporting

### Fault Isolation and Restoration

Fault isolation identifies and isolates faulted equipment to prevent cascading failures.

- Analyze protective relay operations and fault indicators to locate the fault
- Coordinate automatic recloser operations on distribution feeders
- Execute switching sequences to isolate the faulted section while maintaining service to adjacent areas
- Verify isolation through SCADA confirmation before authorizing crew access
- Coordinate restoration switching to return the network to normal configuration after repairs

## Key Aggregates and Entities

- **TransmissionLine** (aggregate root): A high-voltage conductor connecting grid nodes, with thermal limits and impedance characteristics
- **Substation** (aggregate root): A facility containing transformers, switches, and monitoring equipment
- **Outage** (aggregate root): A service interruption event tracked from detection through restoration
- **DistributionFeeder** (aggregate root): A medium-voltage circuit serving end-use customers
- **VoltageRating** (value object): The rated voltage level of a piece of equipment
- **ThermalLimit** (value object): The maximum allowable power flow on a line or transformer
- **LineImpedance** (value object): The electrical characteristics of a transmission line
- **FaultLocation** (value object): The estimated location of a fault on the network

## Domain Events Produced

- **OutageDetected**: A service interruption has been identified
- **FaultIsolated**: A fault has been isolated from the rest of the network
- **ServiceRestored**: Power has been restored to an affected area
- **LineOverloaded**: A transmission line is approaching thermal limits
- **SubstationAlarmTriggered**: An abnormal condition detected at a substation
- **SwitchingOperationCompleted**: A switching sequence has been executed

## Domain Events Consumed

- **UnitDispatched** (from Generation Context): Generation output changes affecting power flow
- **UnitTripped** (from Generation Context): Sudden generation loss requiring grid rebalancing
- **RenewableOutputChanged** (from Renewable Integration Context): Variable renewable output affecting distribution flow
- **DERExporting** (from Renewable Integration Context): Distributed generation affecting feeder power flow direction

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- T&D is one of six core bounded contexts
- [Generation Context](../generation-context/) -- Upstream source of power injections
- [Metering & Billing Context](../metering-billing-context/) -- Downstream consumer of outage duration data
- [Regulatory & Grid Context](../regulatory-grid-context/) -- Downstream consumer of reliability metrics
- [Energy Standards](../energy-standards/) -- IEC 61850 governs substation communication
- [Domain Events](../domain-events/) -- Produces outage, fault, and grid status events

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Glover, J. Duncan, Overbye, Thomas J. & Sarma, Mulukutla S. "Power Systems Analysis and Design." 6th Edition. Cengage, 2017.
- IEC 61850 Standard. "Communication Networks and Systems for Power Utility Automation." International Electrotechnical Commission.
- IEEE Standard 1366. "IEEE Guide for Electric Power Distribution Reliability Indices." IEEE, 2012.
