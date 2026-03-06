# Domain Services in Logistics

## Overview

A domain service is a stateless operation that encapsulates domain logic which does not naturally belong to a single entity or value object. Domain services coordinate behavior across multiple aggregates or perform calculations that span aggregate boundaries. In logistics, domain services model operations like route optimization, carrier matching, customs clearance orchestration, and dock scheduling that require data from multiple aggregates to produce a result.

## Relevance to Logistics

Many logistics operations involve logic that spans multiple aggregates or bounded contexts. Optimizing a route requires knowledge of vehicle capacity, driver availability, and shipment constraints. Matching a load to a carrier requires knowledge of carrier rates, lane coverage, and performance history. Scheduling a dock appointment requires knowledge of trailer arrivals, dock door availability, and loading priorities. Domain services model these cross-aggregate operations as explicit, named concepts in the ubiquitous language.

## Logistics Domain Services

### Fleet Management Context

- **DispatchService**: Assigns the optimal vehicle and driver to a load based on vehicle proximity, driver HOS availability, equipment compatibility, and operational priorities. Coordinates between Vehicle and Driver aggregates.
- **ComplianceCheckService**: Validates that a vehicle and driver combination meets all regulatory requirements (valid inspection, current insurance, driver medical certificate, appropriate endorsements) before authorizing dispatch.

### Route Optimization Context

- **RouteOptimizationService**: Calculates the optimal route for a vehicle given a set of pickup and delivery stops, time windows, vehicle capacity, road restrictions, and real-time traffic conditions. This is the core algorithmic engine of the logistics domain.
- **LoadConsolidationService**: Analyzes multiple pending shipments and determines which can be combined into a single vehicle load to maximize utilization and minimize deadhead miles.
- **ReroutingService**: Recalculates a route in real-time when conditions change (traffic, weather, vehicle breakdown, new stop added), producing an updated route with revised ETAs.

### Freight Brokerage Context

- **CarrierMatchingService**: Evaluates available carriers for a load based on lane coverage, equipment type, rate competitiveness, performance history, and current capacity. Ranks carriers and produces a recommended carrier list.
- **RateCalculationService**: Computes freight rates for a shipment based on distance, weight, freight class, fuel surcharges, accessorial charges, and applicable contract terms.
- **MarginAnalysisService**: Calculates the broker's margin on a load by comparing the shipper's rate to the carrier's rate and factoring in operational costs.

### Customs & Clearance Context

- **CustomsClearanceService**: Orchestrates the end-to-end customs clearance process: validating documentation completeness, filing declarations with the appropriate authority, tracking clearance status, and handling holds or inspections.
- **DutyCalculationService**: Computes import duties and tariffs based on HS code classification, declared value, country of origin, applicable trade agreements, and current tariff schedules.
- **TradeComplianceService**: Screens shipments against denied party lists, embargo restrictions, and export control classifications to ensure regulatory compliance before filing.

### Yard Management Context

- **DockSchedulingService**: Assigns trailers to dock doors based on appointment priority, loading/unloading type, equipment compatibility, and yard congestion. Resolves scheduling conflicts and optimizes dock utilization.
- **YardMoveService**: Plans and directs yard jockey movements to reposition trailers between yard positions and dock doors, minimizing travel time and avoiding congestion.

### Last-Mile Delivery Context

- **DeliveryRouteBuilderService**: Constructs optimized delivery routes from a set of delivery stops, considering time windows, vehicle capacity, driver start location, and traffic patterns.
- **DeliveryDispatchService**: Assigns delivery routes to available drivers based on proximity, vehicle type, delivery area familiarity, and workload balance.

## Domain Service Design Principles

- Domain services are stateless: they do not hold state between invocations
- Domain services are named using verbs or verb phrases from the ubiquitous language
- Domain services operate on aggregates loaded from repositories
- Domain services do not directly access infrastructure (databases, APIs); they use repositories and anti-corruption layers
- If logic naturally belongs to a single entity or value object, place it there instead of creating a service

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Domain services coordinate operations across multiple aggregates
- [Repositories](../repositories/) -- Domain services use repositories to load and save aggregates
- [Entities](../entities/) -- Domain services operate on entities within aggregates
- [Value Objects](../value-objects/) -- Domain services may produce value objects as results
- [Domain Events](../domain-events/) -- Domain services may trigger domain events as side effects
- [Bounded Contexts](../bounded-contexts/) -- Domain services are scoped to a single bounded context

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7: Services. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 5: Implementing Business Logic. O'Reilly, 2021.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 16: Domain Services. Wrox, 2015.
