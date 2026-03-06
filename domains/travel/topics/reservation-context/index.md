# Reservation Context

## Overview

The Reservation Context is the core transactional context in the travel domain, responsible for creating, modifying, and cancelling bookings. The Reservation serves as the aggregate root, encompassing flight segments, hotel stays, car rentals, passenger booking records, and ancillary services. This context owns the PNR lifecycle, ensures booking consistency across volatile external inventory, and coordinates with GDS platforms for confirmation. It emphasizes transactional integrity, state management, and the enforcement of complex booking rules.

## Key Domain Concepts

### Reservation (Aggregate Root)

The central aggregate in the travel domain. A Reservation is identified by a ReservationId and typically maps to a PNR in the GDS. It contains segments, passenger records, contact information, ticketing deadlines, and booking status. All modifications to the booking -- adding segments, associating passengers, issuing tickets, cancelling -- flow through the Reservation aggregate root.

### Segment

A single travel component within a reservation: a flight leg, a hotel night range, or a car rental period. Each segment has its own status (confirmed, waitlisted, cancelled) and links to pricing and inventory in external systems. Segments are entities within the Reservation aggregate.

### Passenger Booking Record

A per-reservation instance linking a Passenger (from the Passenger Management Context) to this specific booking. Contains booking-specific data: ticket number, seat assignment, meal selection, and special service requests for this trip. Distinct from the long-lived Passenger Profile.

### PNR (Passenger Name Record)

The external identifier assigned by the GDS or airline system. The PNR is a six-character alphanumeric code that serves as the universal reference for the booking across the travel industry. The Reservation maintains both its internal ReservationId and the external PNR.

### Ticketing Deadline

A time-limited window by which the reservation must be ticketed (payment confirmed and ticket issued) or it will be automatically cancelled by the airline or GDS. The Reservation aggregate enforces this constraint and publishes ReservationExpired if the deadline passes.

### Booking Lifecycle

- **Created**: Initial reservation with segments and passengers
- **Confirmed**: All segments confirmed with GDS/airline
- **Ticketed**: Tickets issued, payment captured
- **InProgress**: Travel has begun (first segment departed)
- **Completed**: All segments have been traveled
- **Cancelled**: Booking cancelled by passenger, agent, or system
- **Expired**: Ticketing deadline passed without ticketing

## Invariants

- Every reservation must have at least one segment and one passenger
- All passengers must have names in the format required by the operating carrier
- Reservation cannot transition to Ticketed unless all segments are Confirmed and payment is authorized
- Cancellation is not permitted after departure of the first segment (exchange or no-show rules apply instead)
- Ticketing deadline must be respected or the reservation is subject to auto-cancellation
- Ancillary services (seat selection, baggage) cannot be added to cancelled or expired reservations

## Integration Points

- **GDS platforms**: PNR creation, modification, and cancellation via Anti-Corruption Layers
- **Search & Availability Context**: Receives selected offers for booking
- **Pricing & Fare Context**: Requests fare quotes and validates pricing
- **Passenger Management Context**: Retrieves passenger profiles and travel documents
- **Itinerary Context**: Publishes confirmed segments for itinerary assembly
- **Payment & Billing Context**: Triggers payment authorization and capture

## Domain Events

- **ReservationCreated**: New booking created with initial segments and passengers
- **ReservationConfirmed**: All segments confirmed with the provider
- **ReservationTicketed**: Tickets issued for the booking
- **ReservationCancelled**: Booking cancelled
- **ReservationExpired**: Ticketing deadline passed
- **SegmentChanged**: A segment's schedule, status, or details changed
- **AncillaryAdded**: An ancillary service was added to the reservation

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Reservation is the primary aggregate root in the travel domain
- [Entities](../entities/) -- Segments and Passenger Booking Records are entities within the aggregate
- [Domain Events](../domain-events/) -- Reservation state changes drive the booking lifecycle
- [Anti-Corruption Layer](../anti-corruption-layer/) -- GDS communication is mediated through ACLs
- [Search & Availability Context](../search-availability-context/) -- Upstream provider of bookable offers
- [Payment & Billing Context](../payment-billing-context/) -- Downstream consumer for payment processing

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- IATA. "Passenger Services Conference Resolutions Manual (PSCRM)."
- IATA. "AIRIMP: ATA/IATA Reservations Interline Message Procedures."
