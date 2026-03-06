# Generation Context in Energy

## Overview

The Generation Context is the bounded context responsible for managing power generation assets, dispatching generating units to meet demand, optimizing fuel procurement, and scheduling maintenance. It serves as the supply side of the energy domain, translating demand signals and market instructions into physical power production. This context owns the lifecycle of generating units from commissioning through operations to decommissioning.

## Relevance to Energy

Every kilowatt-hour consumed must first be generated. The Generation Context ensures that sufficient, reliable power production is available to meet demand while minimizing cost and environmental impact. It absorbs the complexity of managing diverse generation technologies (thermal, nuclear, hydro, combustion turbine), each with different operating characteristics, fuel requirements, and cost structures, and presents a unified dispatch interface to the grid and market contexts.

## Bounded Context Boundaries

**Owns**: GeneratingUnit aggregate, PowerPlant aggregate, FuelContract aggregate, dispatch scheduling, maintenance planning, heat rate optimization

**Does not own**: Transmission grid operations (Transmission & Distribution Context), market bidding strategies (Trading & Market Context), renewable generation forecasting (Renewable Integration Context)

**Upstream contexts**: Trading & Market Context (dispatch instructions from market clearing), Regulatory & Grid Context (reliability dispatch orders)

**Downstream contexts**: Transmission & Distribution Context (generation output data), Trading & Market Context (available capacity and cost data)

## Core Capabilities

### Unit Dispatch

Unit dispatch directs generating units to produce specific output levels at specific times to meet grid demand.

- Accept dispatch instructions from the Trading & Market Context based on market clearing
- Accept reliability dispatch orders from the Regulatory & Grid Context for grid emergencies
- Calculate the optimal output level for each unit based on heat rate curves and fuel costs
- Respect unit-specific constraints: minimum output, maximum capacity, ramp rate limits, minimum run time, minimum down time
- Coordinate multi-unit plants to optimize combined efficiency
- Track actual vs. dispatched output and report deviations

### Capacity Planning

Capacity planning ensures that sufficient generation resources are available to meet anticipated future demand.

- Maintain an inventory of all generation assets with current capacity ratings
- Track planned retirements, additions, and capacity changes
- Assess seasonal availability considering planned maintenance windows and historical forced outage rates
- Report available capacity to the Trading & Market Context for market participation
- Report capacity commitments to the Regulatory & Grid Context for reliability planning

### Fuel Management

Fuel management ensures that generating units have adequate fuel supply at optimal cost.

- Manage fuel contracts with suppliers (natural gas, coal, oil, uranium)
- Track fuel inventory levels at each generation station
- Forecast fuel consumption based on dispatch schedules and unit heat rates
- Optimize fuel procurement timing and quantity to minimize cost and maintain required inventory levels
- Monitor fuel quality and adjust heat rate calculations accordingly

### Maintenance Scheduling

Maintenance scheduling coordinates planned outages to minimize impact on generation availability and cost.

- Schedule planned maintenance windows for each generating unit based on manufacturer recommendations, regulatory requirements, and operating hours
- Coordinate maintenance across units within a plant to avoid simultaneous outages
- Coordinate with the Regulatory & Grid Context to ensure maintenance does not create reliability violations
- Track maintenance history, component condition, and remaining useful life

## Key Aggregates and Entities

- **GeneratingUnit** (aggregate root): A single power-producing machine with dispatch capability, identified by GeneratingUnitId
- **PowerPlant** (aggregate root): A facility containing one or more generating units at a specific location
- **FuelContract** (aggregate root): An agreement with a fuel supplier specifying fuel type, quantity, price, and delivery schedule
- **MaintenanceSchedule** (entity within GeneratingUnit): Planned maintenance windows and maintenance history
- **Capacity** (value object): The rated maximum output of a generating unit in megawatts
- **HeatRate** (value object): The thermal efficiency of a unit in BTU per kWh
- **RampRate** (value object): The maximum rate of output change in MW per minute
- **OperatingStatus** (value object): The current state of a generating unit (Available, Dispatched, Maintenance, ForcedOutage)

## Domain Events Produced

- **UnitDispatched**: A generating unit has been directed to a specific output level
- **UnitRamped**: A generating unit has changed its output
- **UnitTripped**: A generating unit has unexpectedly gone offline
- **MaintenanceStarted**: A generating unit has entered planned maintenance
- **MaintenanceCompleted**: A generating unit has returned to service
- **CapacityChanged**: A generating unit's rated capacity has been updated
- **FuelDeliveryReceived**: A fuel shipment has been received at a generation station

## Domain Events Consumed

- **DispatchInstructionIssued** (from Trading & Market Context): Market-clearing dispatch instructions
- **ReliabilityDispatchOrdered** (from Regulatory & Grid Context): Emergency dispatch orders for grid reliability
- **DemandForecastUpdated** (from Regulatory & Grid Context): Updated demand forecasts for capacity planning
- **RenewableForecastUpdated** (from Renewable Integration Context): Renewable generation forecasts affecting thermal dispatch needs

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Generation is one of six core bounded contexts
- [Transmission & Distribution Context](../transmission-distribution-context/) -- Downstream consumer of generation output data
- [Trading & Market Context](../trading-market-context/) -- Upstream source of dispatch instructions
- [Renewable Integration Context](../renewable-integration-context/) -- Partner context for integrated dispatch
- [Aggregates](../aggregates/) -- Owns GeneratingUnit, PowerPlant, and FuelContract aggregates
- [Domain Events](../domain-events/) -- Produces dispatch, trip, and maintenance events

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Wood, Allen J. & Wollenberg, Bruce F. "Power Generation, Operation, and Control." 3rd Edition. Wiley, 2013.
- Kirschen, Daniel S. & Strbac, Goran. "Fundamentals of Power System Economics." 2nd Edition. Wiley, 2018.
