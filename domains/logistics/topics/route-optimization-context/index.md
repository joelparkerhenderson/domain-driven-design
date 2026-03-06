# Route Optimization Context in Logistics

## Overview

The Route Optimization Context is the bounded context responsible for planning, optimizing, and managing freight routes from origin to destination. It encompasses multi-stop route calculation, load consolidation, scheduling against time windows, real-time rerouting based on traffic and weather, and estimated time of arrival management. This is typically the most algorithmically complex context in a logistics domain and a core competitive differentiator.

## Relevance to Logistics

Route optimization directly impacts the two largest cost drivers in logistics: fuel and driver time. A 10% improvement in route efficiency across a fleet of 500 trucks can save millions annually. This context must solve the Vehicle Routing Problem (VRP) and its variants -- time-windowed VRP, capacitated VRP, pickup-and-delivery VRP -- under real-world constraints including traffic patterns, road restrictions, driver HOS limits, customer appointment windows, and vehicle capabilities. The challenge is producing near-optimal solutions fast enough for real-time dispatch decisions.

## Bounded Context Boundaries

**Owns**: Route aggregate, LoadPlan aggregate, scheduling algorithms, optimization heuristics, ETA calculations, rerouting logic

**Does not own**: Vehicle and driver details (Fleet Management Context), freight pricing (Freight Brokerage Context), dock scheduling (Yard Management Context), delivery execution (Last-Mile Delivery Context)

**Upstream contexts**: Freight Brokerage Context (provides accepted loads requiring routing), Fleet Management Context (provides vehicle availability and driver status)

**Downstream contexts**: Fleet Management Context (receives route assignments), Yard Management Context (receives ETAs for dock scheduling), Last-Mile Delivery Context (receives delivery route plans)

## Core Capabilities

### Route Planning

- **Multi-stop routing**: Calculating the optimal sequence of stops for a vehicle given pickup and delivery locations, time windows, and vehicle capacity
- **Distance and time calculation**: Computing drive distance and drive time between stops using road network data, accounting for speed limits, road classifications, and truck restrictions
- **Constraint satisfaction**: Ensuring routes respect vehicle weight and dimension limits, driver HOS rules, bridge and tunnel restrictions, and hazmat routing requirements
- **Route costing**: Calculating the total cost of a route including fuel, driver wages, tolls, and opportunity cost of deadhead miles

### Load Optimization

- **Load consolidation**: Combining multiple shipments into a single vehicle load to maximize capacity utilization and reduce per-shipment cost
- **Multi-drop sequencing**: Ordering delivery stops within a route to minimize total distance while respecting time windows and unloading constraints
- **Backhaul identification**: Finding return loads for vehicles that would otherwise deadhead, reducing empty miles
- **Intermodal planning**: Determining when freight should transfer between modes (truck to rail, truck to ocean) based on cost, time, and service requirements

### Real-Time Operations

- **ETA management**: Continuously recalculating estimated arrival times based on actual vehicle position, traffic conditions, and remaining stops
- **Dynamic rerouting**: Automatically recalculating routes when conditions change -- traffic congestion, road closures, weather events, new stop additions, or vehicle breakdowns
- **Geofence-triggered actions**: Initiating events (customer notifications, dock preparation) when a vehicle enters a geofence around a destination
- **Exception handling**: Detecting and escalating route deviations, missed time windows, and unplanned stops

### Scheduling

- **Appointment coordination**: Aligning route timing with dock appointments at pickup and delivery locations
- **Driver shift planning**: Ensuring routes fit within driver HOS availability and mandatory rest break requirements
- **Batch optimization**: Solving for multiple routes simultaneously to achieve system-wide optimization rather than route-by-route optimization

## Key Aggregates and Entities

- **Route** (aggregate root): An ordered sequence of route segments with origin, destination, intermediate stops, timing, and assigned vehicle
- **RouteSegment** (entity within Route): A single leg between two consecutive waypoints with distance, estimated duration, and road-specific constraints
- **LoadPlan** (aggregate root): The assignment of shipments to vehicles for a planning period, ensuring capacity constraints are met
- **TimeWindow** (value object): A start time and end time defining when a pickup or delivery must occur
- **Distance** (value object): A measurement with value and unit for segment and route-level distances
- **Duration** (value object): An elapsed time measurement for transit and dwell calculations

## Domain Events Produced

- **RoutePlanned**: A new route has been calculated and is ready for vehicle/driver assignment
- **RouteOptimized**: An existing route has been recalculated for improved efficiency
- **RouteDeviated**: A vehicle has deviated from its planned route
- **ETAUpdated**: Estimated arrival time at a stop has been recalculated
- **LoadConsolidated**: Multiple shipments have been assigned to a single vehicle

## Domain Events Consumed

- **TenderAccepted** (from Freight Brokerage Context): Triggers route planning for the accepted load
- **VehicleDispatched** (from Fleet Management Context): Activates real-time tracking and ETA management for the route
- **VehiclePlacedOutOfService** (from Fleet Management Context): Triggers rerouting for affected loads

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Route Optimization is one of the six core bounded contexts
- [Fleet Management Context](../fleet-management-context/) -- Partnership for vehicle availability and route assignment
- [Freight Brokerage Context](../freight-brokerage-context/) -- Receives accepted loads for routing
- [Yard Management Context](../yard-management-context/) -- Provides ETAs for dock scheduling
- [Domain Services](../domain-services/) -- RouteOptimizationService, LoadConsolidationService, and ReroutingService operate here
- [Subdomains](../subdomains/) -- Route optimization is a core subdomain

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Toth, Paolo & Vigo, Daniele. "Vehicle Routing: Problems, Methods, and Applications." SIAM, 2014.
- Golden, Bruce, Raghavan, S. & Wasil, Edward. "The Vehicle Routing Problem: Latest Advances and New Challenges." Springer, 2008.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6: Bounded Contexts. O'Reilly, 2021.
