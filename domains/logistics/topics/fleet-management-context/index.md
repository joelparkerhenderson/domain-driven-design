# Fleet Management Context in Logistics

## Overview

The Fleet Management Context is the bounded context responsible for managing the lifecycle of transportation assets (vehicles, tractors, trailers) and personnel (drivers). It encompasses vehicle acquisition and decommissioning, preventive and corrective maintenance, regulatory compliance (inspections, insurance, registrations), driver qualification and hours-of-service management, and real-time telematics data from vehicles in the field.

## Relevance to Logistics

Fleet management is the operational foundation of any asset-based logistics operation. Without well-maintained vehicles and compliant drivers, no freight moves. This context absorbs the complexity of tracking thousands of assets across multiple terminals, managing regulatory requirements from FMCSA/DOT, integrating with ELD hardware providers for real-time driver logs, and making dispatch decisions that balance asset utilization against compliance constraints. Effective fleet management directly reduces operating costs through preventive maintenance, minimizes regulatory risk through compliance tracking, and maximizes revenue through intelligent dispatch.

## Bounded Context Boundaries

**Owns**: Vehicle aggregate, Driver aggregate, maintenance schedules, compliance records, telematics data, dispatch authorization

**Does not own**: Route planning (Route Optimization Context), freight pricing (Freight Brokerage Context), dock scheduling (Yard Management Context), delivery execution (Last-Mile Delivery Context)

**Upstream contexts**: Route Optimization Context (provides route assignments requiring vehicle and driver allocation)

**Downstream contexts**: Route Optimization Context (receives vehicle availability and driver status), Yard Management Context (receives vehicle arrival/departure notifications)

## Core Capabilities

### Vehicle Lifecycle Management

- **Registration and onboarding**: Recording vehicle specifications (VIN, make, model, GVWR, axle configuration, fuel type, trailer compatibility) and regulatory documents (registration, insurance, IFTA decals)
- **Assignment tracking**: Managing which terminal, division, or driver a vehicle is currently assigned to
- **Decommissioning**: Retiring vehicles from active service with full audit trail of lifecycle events

### Maintenance Management

- **Preventive maintenance scheduling**: Tracking maintenance intervals by mileage, engine hours, or calendar time and generating work orders when thresholds are approached
- **Corrective maintenance tracking**: Recording unplanned repairs with root cause, parts used, labor hours, and cost
- **Inspection management**: Tracking FMCSA-required annual inspections, driver pre-trip and post-trip inspections, and state-specific inspections
- **Out-of-service management**: Placing vehicles out of service when defects are found and tracking return-to-service certification

### Driver Management

- **Qualification file maintenance**: Tracking CDL class, endorsements (hazmat, tanker, doubles/triples), medical certificate expiration, MVR (motor vehicle record) review dates, and drug/alcohol test history
- **Hours-of-service tracking**: Consuming ELD data to monitor driving hours, on-duty hours, and required rest breaks under FMCSA 49 CFR Part 395
- **Assignment management**: Tracking driver-to-vehicle assignments, home terminal assignments, and team driving pairs

### Telematics Integration

- **GPS position tracking**: Receiving real-time vehicle positions from telematics devices for visibility and dispatch decisions
- **Engine diagnostics**: Consuming diagnostic trouble codes (DTCs) and engine performance data for predictive maintenance
- **Geofence events**: Triggering alerts when vehicles enter or exit defined geographic boundaries (terminals, customer sites, restricted zones)
- **Fuel consumption monitoring**: Tracking fuel usage by vehicle and route for cost analysis and efficiency optimization

## Key Aggregates and Entities

- **Vehicle** (aggregate root): A transportation asset with specifications, maintenance history, compliance records, and current assignment
- **Driver** (aggregate root): A licensed operator with qualifications, HOS records, certifications, and current assignment
- **MaintenanceRecord** (entity within Vehicle): A specific maintenance event with date, type, parts, labor, and cost
- **InspectionRecord** (entity within Vehicle): A regulatory inspection result with date, inspector, findings, and pass/fail status
- **HOSRecord** (value object): A daily driver log entry with duty status changes, driving time, and on-duty time
- **VehicleSpecification** (value object): Immutable technical specifications of a vehicle
- **GPSCoordinate** (value object): A latitude/longitude position with timestamp

## Domain Events Produced

- **VehicleRegistered**: A new vehicle has been added to the fleet
- **VehicleDispatched**: A vehicle has been authorized for departure on an assigned route
- **VehicleReturnedToYard**: A vehicle has arrived at a terminal
- **MaintenanceScheduled**: Preventive or corrective maintenance has been planned
- **VehiclePlacedOutOfService**: A vehicle has been removed from active service
- **DriverHOSWarning**: A driver is approaching HOS limits
- **DriverQualificationExpiring**: A driver credential is approaching expiration

## Domain Events Consumed

- **RoutePlanned** (from Route Optimization Context): Triggers vehicle and driver allocation for the planned route
- **LoadTendered** (from Freight Brokerage Context): Triggers capacity check for the specified equipment type and lane

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Fleet Management is one of the six core bounded contexts
- [Route Optimization Context](../route-optimization-context/) -- Partnership relationship for vehicle/driver allocation and route assignment
- [Yard Management Context](../yard-management-context/) -- Receives vehicle arrival/departure notifications
- [Domain Services](../domain-services/) -- DispatchService and ComplianceCheckService operate within this context
- [Regulatory Compliance](../regulatory-compliance/) -- FMCSA, DOT, HOS, and ELD regulations shape this context's rules
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ELD provider integrations use ACL adapters

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6: Bounded Contexts. O'Reilly, 2021.
- Federal Motor Carrier Safety Administration. "Federal Motor Carrier Safety Regulations." 49 CFR Parts 390-399. US Department of Transportation.
- Federal Motor Carrier Safety Administration. "Electronic Logging Devices." 49 CFR Part 395. US Department of Transportation.
