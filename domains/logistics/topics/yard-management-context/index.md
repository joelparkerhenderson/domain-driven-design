# Yard Management Context in Logistics

## Overview

The Yard Management Context is the bounded context responsible for coordinating the physical movement and positioning of trailers, containers, and vehicles within a facility's yard -- the outdoor area between the gate and the dock doors. It encompasses gate check-in and check-out, trailer position tracking, dock door scheduling, yard jockey dispatch, and yard inventory management. This context bridges the gap between over-the-road transportation and warehouse or cross-dock operations.

## Relevance to Logistics

Yard congestion is a major source of inefficiency in logistics. Drivers waiting at gates, trailers parked in wrong positions, dock doors sitting idle while loaded trailers wait to be moved, and yard jockeys searching for trailers all consume time and money. Industry estimates put detention costs at $1-2 billion annually in the US alone. A well-modeled yard management context reduces dwell time, increases dock door utilization, eliminates detention charges, and provides real-time visibility into trailer location and status.

## Bounded Context Boundaries

**Owns**: DockAppointment aggregate, TrailerPosition aggregate, GateTransaction aggregate, yard jockey dispatch, yard inventory

**Does not own**: Vehicle maintenance or driver HOS (Fleet Management Context), route planning (Route Optimization Context), freight pricing (Freight Brokerage Context), warehouse operations inside the building

**Upstream contexts**: Route Optimization Context (provides ETAs for inbound vehicles), Fleet Management Context (provides vehicle identification data)

**Downstream contexts**: Last-Mile Delivery Context (notifies when trailers are loaded and ready for delivery dispatch), warehouse systems (notifies when trailers are docked and ready for loading/unloading)

## Core Capabilities

### Gate Operations

- **Check-in processing**: Recording carrier identification, driver credentials, trailer number, seal number, load reference, and appointment confirmation when a vehicle arrives at the gate
- **Check-out processing**: Recording departure time, updated seal number, and load status when a vehicle departs the facility
- **Gate queue management**: Tracking vehicles waiting at the gate and prioritizing check-in based on appointment time and load priority
- **Security verification**: Validating driver identity, carrier authority, and appointment against expected arrivals

### Dock Door Scheduling

- **Appointment management**: Creating, modifying, and cancelling dock door appointments with specific time slots, door assignments, and load types (inbound, outbound, live load, drop)
- **Appointment optimization**: Scheduling appointments to maximize dock door utilization while minimizing carrier wait time and avoiding bottlenecks
- **Priority scheduling**: Managing appointment priority based on load urgency, carrier type (owner-operator vs. fleet), and customer SLA requirements
- **Conflict resolution**: Detecting and resolving scheduling conflicts when appointments overlap or dock doors become unavailable

### Trailer Tracking

- **Position management**: Recording and updating the physical location of every trailer in the yard -- which row, slot, or zone it occupies
- **Status tracking**: Monitoring trailer status: empty, loaded, partially loaded, sealed, being loaded, being unloaded, ready for dispatch
- **Content tracking**: Maintaining a summary of trailer contents including load reference, commodity type, and destination
- **Dwell time monitoring**: Tracking how long each trailer has been in the yard and alerting when dwell time exceeds thresholds

### Yard Jockey Operations

- **Move requests**: Creating and prioritizing requests to move trailers between yard positions and dock doors
- **Jockey dispatch**: Assigning move requests to available yard jockeys based on proximity, priority, and workload balance
- **Move confirmation**: Recording move completion with timestamp, origin position, and destination position
- **Jockey productivity tracking**: Monitoring moves per hour, average move time, and idle time for yard jockeys

## Key Aggregates and Entities

- **DockAppointment** (aggregate root): A scheduled time slot at a specific dock door for a carrier, with load reference, appointment status, and actual arrival/departure times
- **TrailerPosition** (aggregate root): The current yard location and status of a trailer, including position coordinates, seal status, and content summary
- **GateTransaction** (aggregate root): A check-in or check-out event at the facility gate with vehicle, driver, trailer, and timestamp data
- **DockDoor** (entity): A physical loading/unloading bay with door number, type (inbound, outbound, both), and equipment compatibility
- **YardPosition** (value object): A named location in the yard (zone, row, slot)
- **SealNumber** (value object): A tamper-evident seal identifier
- **AppointmentSlot** (value object): A date, time range, and dock door defining an available window

## Domain Events Produced

- **TrailerCheckedIn**: A trailer has entered the yard through the gate
- **TrailerCheckedOut**: A trailer has departed the yard through the gate
- **DockAppointmentScheduled**: A dock door time slot has been reserved
- **TrailerDockedAtDoor**: A trailer has been positioned at a dock door
- **LoadingCompleted**: A trailer has been fully loaded and sealed
- **UnloadingCompleted**: A trailer has been fully unloaded
- **TrailerRepositioned**: A trailer has been moved to a new yard position

## Domain Events Consumed

- **ETAUpdated** (from Route Optimization Context): Updates expected arrival time for pre-scheduling dock doors
- **VehicleReturnedToYard** (from Fleet Management Context): Triggers gate check-in processing
- **RoutePlanned** (from Route Optimization Context): Provides inbound trailer information for appointment scheduling

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Yard Management is one of the six core bounded contexts
- [Route Optimization Context](../route-optimization-context/) -- Receives ETAs for dock scheduling
- [Fleet Management Context](../fleet-management-context/) -- Receives vehicle identification for gate processing
- [Last-Mile Delivery Context](../last-mile-delivery-context/) -- Notifies when trailers are loaded for delivery
- [Domain Services](../domain-services/) -- DockSchedulingService and YardMoveService operate here
- [Domain Events](../domain-events/) -- Yard events trigger downstream operations

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6: Bounded Contexts. O'Reilly, 2021.
- Bartholdi, John J. & Hackman, Steven T. "Warehouse and Distribution Science." Supply Chain and Logistics Institute, Georgia Tech, 2019.
