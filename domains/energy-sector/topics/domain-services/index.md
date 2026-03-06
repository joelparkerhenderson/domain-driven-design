# Domain Services in Energy

## Overview

A domain service is a stateless operation that encapsulates domain logic which does not naturally belong to any single entity or value object. Domain services handle operations that span multiple aggregates, require coordination between domain concepts, or implement complex business rules that do not fit within a single aggregate's responsibility. In the energy domain, domain services orchestrate dispatch optimization, settlement calculation, forecasting, and compliance assessment.

## Relevance to Energy

Many energy operations inherently span multiple aggregates. Dispatch optimization must consider all available generating units, their costs, ramp constraints, and grid transmission limits simultaneously. Settlement calculation must reconcile actual generation, consumption, and market prices across multiple market participants. These cross-aggregate operations belong in domain services rather than being forced into a single aggregate.

## Energy Domain Services

### Generation Context Services

- **DispatchService**: Determines optimal generation dispatch across all available generating units to meet forecasted demand at minimum cost while respecting unit constraints (ramp rates, minimum output, heat rate curves) and grid constraints (transmission limits, reliability requirements). Inputs: demand forecast, available units, fuel costs, grid constraints. Outputs: dispatch schedule with per-unit output assignments.
- **MaintenanceSchedulingService**: Coordinates planned maintenance windows across generating units to ensure that sufficient generation capacity remains available. Considers unit maintenance requirements, seasonal demand patterns, and fuel contract obligations.
- **HeatRateOptimizationService**: Calculates the most efficient operating point for a thermal generating unit based on its heat rate curve, current fuel costs, and market prices.

### Transmission & Distribution Context Services

- **PowerFlowAnalysisService**: Calculates the steady-state power flow through the transmission network given current generation injections and load withdrawals. Identifies potential overloads, voltage violations, and congestion points.
- **OutageManagementService**: Coordinates outage response by correlating SCADA alarms, customer reports, and field crew availability to determine affected areas, prioritize restoration, and estimate restoration times.
- **ContingencyAnalysisService**: Evaluates the impact of potential equipment failures (N-1, N-2 contingencies) on grid reliability and identifies preventive actions.
- **FaultLocalizationService**: Analyzes fault indicators, protective relay operations, and SCADA data to identify the location and type of a grid fault.

### Trading & Market Context Services

- **SettlementService**: Calculates financial settlement amounts by reconciling scheduled vs. actual generation, scheduled vs. actual consumption, and market prices over a settlement period. Handles day-ahead and real-time market settlement, imbalance charges, and uplift allocations.
- **MarketBiddingService**: Formulates optimal bid strategies for day-ahead and real-time markets based on generation portfolio costs, forecasted demand, price forecasts, and risk tolerances.
- **RiskAssessmentService**: Evaluates the market risk exposure of the trading portfolio, calculating value-at-risk, stress test scenarios, and position limit compliance.
- **PriceForecastingService**: Generates price forecasts for energy markets using historical prices, demand forecasts, fuel costs, and generation availability.

### Metering & Billing Context Services

- **MeterDataValidationService**: Validates incoming meter readings against historical patterns, physical constraints (non-negative consumption, maximum demand), and cross-meter checks. Flags anomalous readings for review and applies estimation rules for missing data.
- **BillingCalculationService**: Calculates customer charges by applying tariff rates to validated consumption data, including energy charges, demand charges, fixed charges, taxes, and credits (net metering, demand response).
- **TariffApplicationService**: Determines the applicable tariff for a customer based on their service class, usage pattern, and regulatory tariff schedule. Handles tariff transitions and proration.

### Regulatory & Grid Context Services

- **ComplianceAssessmentService**: Evaluates the organization's compliance status against NERC reliability standards, FERC reporting requirements, and state regulatory mandates. Identifies gaps and generates compliance action items.
- **DemandResponseDispatchService**: Determines which demand response resources to activate during a grid emergency or high-price period, considering program rules, participant availability, and required load reduction.
- **AncillaryServiceAllocationService**: Allocates ancillary service obligations (frequency regulation, spinning reserve, voltage support) across qualified generating units based on capability and cost.

### Renewable Integration Context Services

- **GenerationForecastingService**: Produces renewable generation forecasts by combining weather data (irradiance, wind speed, temperature) with asset-specific models (panel orientation, turbine power curves, historical performance). Outputs probabilistic forecasts with confidence intervals.
- **CurtailmentDecisionService**: Determines whether and how much renewable generation to curtail based on grid congestion, minimum thermal generation requirements, and market conditions. Minimizes curtailed energy while maintaining grid stability.
- **StorageOptimizationService**: Determines optimal charge and discharge schedules for battery storage systems based on market prices, renewable generation forecasts, grid conditions, and battery degradation models.
- **RECTrackingService**: Manages the lifecycle of renewable energy certificates from generation through registration, transfer, and retirement, ensuring accurate tracking and preventing double-counting.

## Domain Service Design Principles

- **Stateless**: Domain services do not maintain state between calls. All necessary data is provided as input or retrieved from repositories.
- **Named with domain verbs**: Service methods are named using domain actions (calculateSettlement, optimizeDispatch, validateMeterReading), not technical terms.
- **Coordinate, do not own**: Domain services coordinate operations across aggregates but do not own aggregate state. The SettlementService calculates amounts but the Settlement aggregate owns the result.
- **Pure domain logic**: Domain services contain no infrastructure concerns (no database access, no HTTP calls). They receive data from repositories and return results to the application layer.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Domain services coordinate operations across multiple aggregates
- [Repositories](../repositories/) -- Domain services use repositories to load aggregates
- [Domain Events](../domain-events/) -- Domain services may trigger domain events as a result of their operations
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context defines its own domain services
- [Value Objects](../value-objects/) -- Domain services operate on and return value objects

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7: Services. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 8: Architectural Patterns. O'Reilly, 2021.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
