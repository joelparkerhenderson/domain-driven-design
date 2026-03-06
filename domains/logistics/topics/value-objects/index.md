# Value Objects in Logistics

## Overview

A value object is an immutable domain object defined entirely by its attributes, with no conceptual identity. Two value objects with the same attributes are considered equal. In logistics, value objects describe measurements, classifications, coordinates, monetary amounts, time windows, and standardized codes that characterize shipments, routes, vehicles, and regulatory filings.

## Relevance to Logistics

Logistics is dense with standardized measurements and classifications. A GPS coordinate (latitude 40.7128, longitude -74.0060) is the same coordinate regardless of which vehicle reported it. A freight class (NMFC Class 70) has the same pricing implications wherever it appears. An HS code (8471.30) classifies a commodity identically at any customs authority. Value objects encode these domain concepts with built-in validation, ensuring that invalid measurements, malformed codes, or inconsistent calculations cannot propagate through the system.

## Key Value Objects by Bounded Context

### Fleet Management Context

- **GPSCoordinate**: Latitude and longitude pair representing a vehicle's position. Validated for valid geographic ranges.
- **VehicleSpecification**: Gross vehicle weight rating, axle count, fuel type, and trailer compatibility. Immutable snapshot of vehicle capabilities.
- **MaintenanceInterval**: Mileage or calendar interval defining when preventive maintenance is due.
- **HOSWindow**: A time span representing remaining available driving hours and duty hours for a driver under HOS rules.

### Route Optimization Context

- **Distance**: A measurement with value and unit (miles or kilometers). Supports conversion between units.
- **Duration**: An elapsed time measurement used for transit time and dwell time calculations.
- **TimeWindow**: A start time and end time defining a pickup window or delivery window. Validates that start precedes end.
- **GeoFence**: A polygon or radius-based geographic boundary for triggering location-based events.
- **Weight**: Mass measurement with value and unit (pounds or kilograms) used for load planning and capacity checks.

### Freight Brokerage Context

- **FreightRate**: A monetary amount per unit (per mile, per hundredweight, flat rate) for a specific lane and service.
- **FreightClass**: NMFC classification (Class 50 through Class 500) based on commodity density, handling, stowability, and liability.
- **Lane**: An origin-destination pair defined by city, state, or zip code region. Two lanes with the same origin and destination are equal.
- **Money**: A monetary amount with currency code. Used for rates, accessorial charges, and invoicing.
- **BillOfLading**: A structured document reference containing shipper, consignee, commodity description, and shipment terms.

### Customs & Clearance Context

- **HSCode**: A harmonized system classification code validated against the HS hierarchy (chapter, heading, subheading).
- **Incoterm**: A standardized trade term (FOB, CIF, DDP, DAP, etc.) defining responsibilities between buyer and seller.
- **DutyAmount**: A calculated duty or tariff amount based on declared value, HS code, and country of origin.
- **CountryOfOrigin**: An ISO 3166 country code identifying where goods were manufactured or substantially transformed.
- **CustomsValue**: A declared monetary value of goods for customs assessment, with currency and valuation method.

### Yard Management Context

- **YardPosition**: A named location within a yard (row, slot, zone) identifying where a trailer is parked or staged.
- **SealNumber**: A tamper-evident seal identifier recorded at gate check-in and verified at dock doors.
- **AppointmentSlot**: A date, time range, and dock door number defining an available loading or unloading window.

### Last-Mile Delivery Context

- **DeliveryAddress**: A structured address with street, city, state, postal code, country, and optional delivery instructions.
- **DeliveryTimeWindow**: A customer-facing time range during which delivery is expected.
- **SignatureImage**: A captured signature as binary data with timestamp, used as proof of delivery.
- **PhotoEvidence**: A captured photograph with timestamp and GPS coordinate, used as delivery confirmation.

## Value Object Design Principles

- Value objects are immutable: once created, their attributes do not change
- Equality is based on all attributes, not on identity
- Value objects encapsulate validation: an invalid value cannot be instantiated
- Value objects encapsulate behavior: conversion, comparison, and formatting logic lives on the value object
- Replace primitive types (strings, numbers) with domain-specific value objects to prevent errors

## Relationships to Other Topics

- [Entities](../entities/) -- Entities contain value objects as descriptive attributes
- [Aggregates](../aggregates/) -- Aggregates contain value objects within their consistency boundary
- [Logistics Standards](../logistics-standards/) -- Standards define validation rules for FreightClass, HSCode, and other value objects
- [Ubiquitous Language](../ubiquitous-language/) -- Value object names come from the ubiquitous language
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate external data into internal value objects

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6: Value Objects. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 5: Implementing Business Logic. O'Reilly, 2021.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 15: Value Objects. Wrox, 2015.
