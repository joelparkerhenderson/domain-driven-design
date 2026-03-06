# Renewable Integration Context in Energy

## Overview

The Renewable Integration Context is the bounded context responsible for managing variable renewable generation assets (solar, wind), battery energy storage systems, distributed energy resources (DERs), generation forecasting, curtailment management, and renewable energy certificate (REC) tracking. It addresses the unique challenges of integrating non-dispatchable, weather-dependent generation into a grid designed for dispatchable thermal resources.

## Relevance to Energy

The rapid growth of renewable energy fundamentally changes how the grid operates. Unlike thermal generators that produce power on demand, solar and wind output varies with weather conditions and cannot be dispatched up at will. Battery storage adds time-shifting capability but introduces its own operational constraints (state of charge, cycle degradation). The Renewable Integration Context manages this variability, providing generation forecasts, curtailment decisions, and storage optimization that enable reliable grid operation with high renewable penetration.

## Bounded Context Boundaries

**Owns**: RenewableAsset aggregate, BatteryStorageSystem aggregate, DERPortfolio aggregate, GenerationForecast aggregate, curtailment management, REC lifecycle, storage dispatch optimization

**Does not own**: Grid power flow (Transmission & Distribution Context), market bidding (Trading & Market Context), customer net metering billing (Metering & Billing Context), renewable portfolio standard compliance (Regulatory & Grid Context)

**Upstream contexts**: External weather data providers (forecasting inputs), equipment manufacturers (asset specifications)

**Downstream contexts**: Generation Context (renewable forecasts for integrated dispatch), Trading & Market Context (forecast and REC data for trading), Metering & Billing Context (DER generation for net metering), Regulatory & Grid Context (REC data for RPS compliance)

## Core Capabilities

### Renewable Asset Management

Renewable asset management tracks the lifecycle and performance of solar, wind, and other renewable generation facilities.

- Register and onboard new renewable assets with specifications (rated capacity, technology type, location, interconnection point)
- Track real-time generation output and compare against rated capacity to calculate capacity factors
- Monitor equipment health indicators (inverter status, turbine vibration, panel degradation)
- Track asset performance over time to identify degradation trends and maintenance needs
- Manage the interconnection relationship between each asset and the grid
- Support repowering scenarios where aging equipment is replaced with newer technology

### Generation Forecasting

Generation forecasting predicts future renewable output to support dispatch planning and market operations.

- Produce solar generation forecasts using irradiance forecasts, panel orientation, temperature, and historical performance
- Produce wind generation forecasts using wind speed forecasts, turbine power curves, and historical performance
- Generate forecasts at multiple time horizons: intra-hour (5-15 minutes), intra-day (1-6 hours), day-ahead (24-48 hours), and seasonal
- Provide probabilistic forecasts with confidence intervals reflecting forecast uncertainty
- Combine individual asset forecasts into portfolio-level forecasts for aggregated trading and dispatch
- Continuously evaluate forecast accuracy and refine models using actual generation data

### Battery Storage Management

Battery storage management optimizes the charge and discharge of battery energy storage systems.

- Track battery state of charge (SOC), charge rate, discharge rate, and cycle count in real time
- Enforce operating constraints: minimum and maximum SOC, maximum charge and discharge rates, simultaneous charge/discharge prohibition
- Optimize charge and discharge schedules based on market prices, renewable generation forecasts, and grid conditions
- Model battery degradation from cycling to inform economic dispatch decisions
- Support multiple use cases: peak shaving, renewable smoothing, frequency regulation, and energy arbitrage
- Coordinate with the Generation Context for integrated dispatch and with the Trading Context for market participation

### DER Management

DER management aggregates and coordinates distributed energy resources across a utility's service territory.

- Register and track DER installations (rooftop solar, behind-the-meter storage, small wind)
- Aggregate DER output data for grid planning and operations
- Model the net impact of DER generation and storage on distribution feeder loading
- Support virtual power plant (VPP) concepts by aggregating DERs for market participation or demand response
- Track DER interconnection agreements and export limits

### Curtailment Management

Curtailment management determines when and how much renewable generation to reduce.

- Evaluate curtailment need based on grid congestion, minimum thermal generation requirements, and oversupply conditions
- Calculate optimal curtailment allocation across renewable assets based on cost, location, and contractual obligations
- Issue curtailment instructions to asset operators or automated control systems
- Track curtailed energy for financial settlement and regulatory reporting
- Minimize total curtailment while maintaining grid stability and reliability

### REC Tracking

REC tracking manages the lifecycle of renewable energy certificates.

- Generate RECs when renewable generation is metered and verified
- Register RECs with applicable tracking systems (WREGIS, M-RETS, NEPOOL GIS, PJM GATS)
- Track REC ownership through transfers and trades
- Retire RECs against renewable portfolio standard obligations or voluntary commitments
- Prevent double-counting by enforcing uniqueness across tracking systems

## Key Aggregates and Entities

- **RenewableAsset** (aggregate root): A solar, wind, or other renewable generation facility, identified by RenewableAssetId
- **BatteryStorageSystem** (aggregate root): A battery installation with charge/discharge capability, identified by BatteryId
- **DERPortfolio** (aggregate root): A collection of distributed energy resources aggregated for management purposes
- **GenerationForecast** (aggregate root): A prediction of future renewable output for a specific asset or portfolio
- **StateOfCharge** (value object): The current energy level of a battery as a percentage
- **RatedCapacity** (value object): The maximum output of a renewable asset in megawatts
- **ForecastHorizon** (value object): The time range and granularity of a generation forecast
- **RenewableEnergyCertificate** (value object): A certificate representing one MWh of renewable generation

## Domain Events Produced

- **GenerationForecastUpdated**: A renewable forecast has been revised
- **CurtailmentApplied**: Renewable output has been intentionally reduced
- **CurtailmentLifted**: A curtailment order has been removed
- **BatteryChargingStarted**: A battery has begun charging
- **BatteryDischargingStarted**: A battery has begun discharging
- **SOCThresholdReached**: A battery has reached a configured SOC threshold
- **RECGenerated**: A renewable energy certificate has been created
- **AssetCommissioned**: A new renewable asset has begun commercial operation
- **DERExporting**: A distributed resource is exporting power to the grid

## Domain Events Consumed

- **MarketPricePublished** (from Trading & Market Context): Prices for storage dispatch optimization
- **LineOverloaded** (from Transmission & Distribution Context): Grid congestion triggering curtailment
- **DemandResponseActivated** (from Regulatory & Grid Context): DR events affecting DER dispatch
- **DispatchInstructionIssued** (from Trading & Market Context): Market dispatch affecting storage

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Renewable Integration is one of six core bounded contexts
- [Generation Context](../generation-context/) -- Partner context for integrated dispatch planning
- [Trading & Market Context](../trading-market-context/) -- Downstream consumer of forecasts and REC data
- [Metering & Billing Context](../metering-billing-context/) -- Downstream consumer of DER generation data
- [Regulatory & Grid Context](../regulatory-grid-context/) -- Downstream consumer of REC data for RPS compliance
- [Energy Standards](../energy-standards/) -- OpenADR, IEEE 1547 for DER interconnection

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Ackermann, Thomas. "Wind Power in Power Systems." 2nd Edition. Wiley, 2012.
- Perez, Richard et al. "Solar Resource Assessment and Forecasting." Elsevier, 2013.
- IEEE 1547 Standard. "Standard for Interconnection and Interoperability of Distributed Energy Resources." IEEE, 2018.
- Ela, Erik et al. "Operating Reserves and Variable Generation." NREL Technical Report NREL/TP-5500-51978, 2011.
