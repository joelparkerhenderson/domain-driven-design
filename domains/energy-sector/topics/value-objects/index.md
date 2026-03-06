# Value Objects in Energy

## Overview

A value object is an immutable domain object defined entirely by its attributes rather than by a unique identity. Two value objects with the same attributes are considered equal and interchangeable. Value objects encapsulate domain concepts that describe, quantify, or measure characteristics of entities and aggregates. In the energy domain, value objects represent measurements, prices, rates, physical quantities, and other descriptive data fundamental to energy operations.

## Relevance to Energy

Energy systems are measurement-intensive. Power output is measured in megawatts, energy in megawatt-hours, voltage in kilovolts, frequency in hertz, and prices in dollars per megawatt-hour. Each of these measurements carries units, precision requirements, and validation rules. Value objects encapsulate these domain concepts with their invariants, preventing invalid measurements (negative megawatts, frequency outside safe range) from entering the domain model.

## Energy Value Objects

### Physical Measurements

- **MegawattHour (MWh)**: A quantity of energy. Must be non-negative. Used in trading, billing, and generation tracking. Equality is based on the numeric value.
- **Megawatt (MW)**: A quantity of power (instantaneous rate of energy). Must be non-negative for generation, may be negative for load. Used in dispatch and capacity planning.
- **Voltage**: An electrical potential measurement in kilovolts (kV). Includes the voltage level and must fall within acceptable operating ranges.
- **GridFrequency**: The AC frequency of the electric grid, nominally 60 Hz (North America) or 50 Hz (Europe/Asia). Must be within allowable deviation bands.
- **PowerFactor**: The ratio of real power to apparent power, ranging from 0 to 1. Indicates efficiency of power usage.
- **LineImpedance**: The resistance, reactance, and impedance characteristics of a transmission line, expressed in ohms per mile.

### Financial Values

- **LMP (Locational Marginal Price)**: A price at a specific grid node at a specific time, expressed in dollars per MWh. Composed of energy, congestion, and loss components.
- **TariffRate**: A rate structure defining charges for electricity consumption, including energy charges, demand charges, and fixed charges.
- **ContractPrice**: The agreed price for an energy contract, including price type (fixed, index, or formula-based) and currency.
- **SettlementAmount**: A calculated financial amount resulting from settlement, including the basis (actual vs. scheduled), period, and currency.
- **RenewableEnergyCertificateValue**: The market value of a REC, including vintage year, technology type, and certification standard.

### Temporal Values

- **BillingPeriod**: A date range defining a billing cycle, including start date, end date, and number of days. Two billing periods with the same dates are identical.
- **DispatchInterval**: A time window (typically 5 or 15 minutes) during which a dispatch instruction is valid. Includes start time, end time, and interval type.
- **ForecastHorizon**: A time range for which a generation or load forecast applies, including granularity (hourly, 15-minute) and confidence interval.

### Operational Values

- **HeatRate**: The thermal efficiency of a generating unit, expressed in BTU per kWh. Must be positive. Lower values indicate higher efficiency.
- **RampRate**: The maximum rate at which a generating unit can change output, expressed in MW per minute. Must be non-negative.
- **StateOfCharge**: The current energy level of a battery system as a percentage (0-100%) of total capacity.
- **CapacityFactor**: The ratio of actual energy produced to maximum possible energy, expressed as a percentage.
- **LoadProfile**: A time series of power consumption values at regular intervals, typically used for billing and forecasting.

### Spatial Values

- **Coordinates**: Geographic latitude and longitude identifying the location of a generation asset, substation, or meter.
- **GridNode**: A named point on the transmission network where power is injected or withdrawn, identified by node name and voltage level.
- **ServiceTerritory**: A geographic boundary defining the area served by a utility or balancing authority.

## Value Object Design Principles

- **Immutability**: Value objects are never modified after creation. To change a price, create a new LMP instance with the updated value.
- **Self-validation**: Value objects validate their data at construction time. A MegawattHour rejects negative values. A GridFrequency rejects values outside the allowable range.
- **Equality by value**: Two LMP objects with the same price, node, and timestamp are equal, regardless of how they were created.
- **Side-effect-free operations**: Value objects can provide operations (e.g., MegawattHour.add(other)) that return new value objects rather than modifying the original.
- **Unit safety**: Energy-specific value objects prevent unit confusion. You cannot add a Megawatt to a MegawattHour without conversion.

## Relationships to Other Topics

- [Entities](../entities/) -- Entities contain value objects as descriptive attributes
- [Aggregates](../aggregates/) -- Value objects live within aggregate boundaries
- [Domain Events](../domain-events/) -- Domain events carry value objects as payload data
- [Ubiquitous Language](../ubiquitous-language/) -- Value object names use domain terminology
- [Energy Standards](../energy-standards/) -- Standards define formats and ranges for many value objects

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6: Value Objects. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 8: Architectural Patterns. O'Reilly, 2021.
