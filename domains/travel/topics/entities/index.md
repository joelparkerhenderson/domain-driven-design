# Entities in Travel

## Overview

An entity is a domain object defined primarily by its unique identity rather than its attributes. Two entities with the same attributes but different identities are distinct objects. Entities have a lifecycle -- they are created, modified over time, and eventually archived or deleted. Their identity persists even as their attributes change. In travel, entities represent the core objects that are tracked, referenced, and modified throughout the booking lifecycle.

## Relevance to Travel

Travel systems must track objects across long-running processes. A passenger exists before, during, and after a trip, accumulating travel history and loyalty status. A flight instance is identified by its carrier, flight number, and date -- even if the departure time, gate, or aircraft type changes. A reservation persists from creation through ticketing, travel, and post-travel settlement. Entity identity enables this tracking across time and system boundaries.

## Travel Entities

### Reservation

The central entity in the booking lifecycle. Identified by a ReservationId (and often an external PNR). A reservation's attributes change as segments are added, passengers are associated, tickets are issued, and status transitions occur, but its identity remains constant throughout.

### Passenger

A traveler identified by a PassengerProfileId. The passenger's name, contact information, document details, and preferences may change over time, but the passenger identity persists across multiple reservations and trips. Within a single reservation, a PassengerBookingRecord links a Passenger to the booking.

### Flight Instance

A specific occurrence of a scheduled flight on a particular date. Identified by the combination of carrier code, flight number, and departure date (e.g., UA-100 on 2026-03-15). The departure time, gate assignment, aircraft type, and status (on-time, delayed, cancelled) may change, but the flight instance identity remains stable.

### Ticket

An electronic travel document identified by a TicketNumber (a 13-digit number per IATA standards). A ticket is associated with a passenger and contains one or more coupons corresponding to flight segments. The ticket's status changes (open, used, refunded, exchanged) but its identity is permanent.

### Travel Document

A passport, visa, or national ID identified by its document number and issuing country. The document's expiration date is checked against travel dates, and the document may be replaced by a renewal, but each document instance has its own identity.

### Loyalty Membership

A frequent flyer or hotel loyalty program membership identified by the program code and membership number. The membership's tier status, points balance, and benefits change over time, but the membership identity persists.

### Hotel Property

A specific hotel identified by a property code. The hotel's room inventory, rates, and availability change constantly, but the property identity is stable. Room types within a property are also entities with distinct identities.

### Car Rental Location

A car rental pickup/dropoff location identified by a location code. The available vehicle inventory changes, but the location identity and its operating hours, address, and capabilities persist.

## Entity Identity Strategies

- **Reservation**: System-generated ReservationId plus GDS-assigned PNR (external identity)
- **Passenger**: System-generated PassengerProfileId; natural identity via passport number is insufficient because documents expire and are reissued
- **Flight Instance**: Composite natural key of carrier code + flight number + departure date
- **Ticket**: IATA-standard 13-digit ticket number, globally unique
- **Travel Document**: Composite of document type + document number + issuing country

## Entity vs. Value Object Decisions

Some travel concepts could be modeled as either entities or value objects depending on context:

- **Seat Assignment**: An entity when tracked over time (assigned, changed, checked-in), a value object when used as a simple attribute of a boarding pass
- **Hotel Room**: An entity in the hotel operations context (Room 405 has maintenance history), potentially a value object in the reservation context (a "Standard King" room type)
- **Segment**: An entity within the Reservation aggregate (it has lifecycle and status), but segment details are often passed as value objects across context boundaries

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Entities serve as aggregate roots or exist within aggregates
- [Value Objects](../value-objects/) -- Entities contain value objects; entity vs. value object is a key modeling decision
- [Domain Events](../domain-events/) -- Entity state changes trigger domain events
- [Repositories](../repositories/) -- Repositories retrieve and persist entities (via aggregate roots)
- [Passenger Management Context](../passenger-management-context/) -- Passenger is a primary entity in this context

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5: Entities. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Entities. O'Reilly, 2021.
