# Logistics - Domain-Driven Design (DDD)

Logistics - Domain-Driven Design (DDD) applies strategic and tactical DDD patterns
to the transportation, fleet management, route optimization, freight brokerage,
customs clearance, yard management, and last-mile delivery domain. Logistics is
distinct from fulfillment: where fulfillment focuses on warehouse-to-customer
order processing, logistics focuses on the movement of goods across transportation
networks, fleet operations, and regulatory compliance for freight.

## Key DDD Concepts Applied to Logistics

- Bounded Context: Logistics is decomposed into six bounded contexts -- Fleet Management, Route Optimization, Freight Brokerage, Customs & Clearance, Yard Management, and Last-Mile Delivery -- each with its own model, language, and consistency boundaries.

- Ubiquitous Language: The terminology used in code (e.g., Shipment, Consignment, BillOfLading, FreightRate, RouteSegment) must match the language used by dispatchers, fleet managers, freight brokers, customs agents, and yard operators.

- Aggregates & Roots: A Shipment or FleetVehicle often acts as an Aggregate Root. This ensures that all associated data (route segments, driver assignments, load manifests) remains consistent and is updated as a single transaction.

- Domain Events: When a critical action occurs -- such as VehicleDispatched, RouteDeviated, CustomsCleared, or DeliveryCompleted -- a domain event is published. This allows other contexts (like Fleet Management or Freight Brokerage) to react without tight coupling.

- Anti-Corruption Layer (ACL): When the logistics system communicates with external ELD providers, carrier APIs, customs authorities (CBP, ACI), or TMS platforms, an ACL translates external data structures into the internal domain model.

## Modeling Logistics Components

- Entities: Vehicle, Driver, Shipment, Route, Carrier, Broker, CustomsDeclaration, DockDoor, Trailer.

- Value Objects: GPSCoordinate, FreightClass, BillOfLading, HSCode, Incoterm, HoursOfServiceWindow, DeliveryTimeWindow.

- Repositories: VehicleRepository, ShipmentRepository, RouteRepository, CarrierRepository.

- Domain Services: RouteOptimizationService, CarrierMatchingService, CustomsClearanceService, DockSchedulingService.

## Distinction from Fulfillment

Fulfillment focuses on order processing, inventory allocation, warehouse pick/pack/ship, and returns. Logistics focuses on the transportation network itself: how vehicles move goods between origins and destinations, how routes are planned and optimized, how freight is brokered and priced, how customs clearance is managed, how yards coordinate trailer movements, and how last-mile delivery reaches the end recipient.
