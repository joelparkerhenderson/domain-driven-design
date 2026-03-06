# Domain Events in Travel

## Overview

A domain event represents something significant that happened in the domain. Domain events are named in past tense (e.g., ReservationConfirmed, PaymentCaptured) and carry the data needed for other parts of the system to react. They enable loose coupling between bounded contexts: the producer publishes the event without knowing who consumes it, and consumers react independently. In travel, domain events are the primary mechanism for coordinating the multi-step booking lifecycle across contexts.

## Relevance to Travel

The travel booking lifecycle spans multiple contexts and external systems. When a passenger completes a purchase, the system must confirm the reservation with the GDS, issue tickets, process payment, generate an itinerary, send notifications, and update loyalty accounts. Tight coupling between these steps creates fragile, untestable systems. Domain events decouple these responsibilities: the Reservation Context publishes `OrderConfirmed`, and each downstream context reacts independently.

## Travel Domain Events

### Reservation Context Events

- **ReservationCreated**: A new reservation has been created with initial segments and passengers. Triggers fare quote validation and passenger document checks.
- **ReservationConfirmed**: Segments are confirmed with the GDS/airline. Triggers itinerary generation and payment authorization.
- **ReservationTicketed**: Tickets have been issued for the reservation. Triggers itinerary finalization and confirmation notification.
- **ReservationCancelled**: The reservation has been cancelled. Triggers refund processing, itinerary removal, and loyalty reversal.
- **ReservationExpired**: The ticketing deadline passed without ticketing. Triggers inventory release and passenger notification.
- **SegmentChanged**: A flight time, route, or status changed (often due to airline schedule change). Triggers itinerary update and passenger notification.

### Pricing & Fare Context Events

- **FareQuoted**: A fare has been calculated and presented to the user. Carries fare components, taxes, and validity period.
- **FareExpired**: A quoted fare is no longer valid. Triggers re-pricing if the booking is still in progress.
- **FareRuleViolated**: A modification would violate fare rules (e.g., minimum stay not met). Triggers rebooking or penalty assessment.

### Passenger Management Context Events

- **PassengerProfileCreated**: A new passenger profile has been registered.
- **DocumentAdded**: A travel document has been added to a passenger profile. May unblock pending reservations waiting for document validation.
- **DocumentExpiring**: A travel document will expire within a configurable threshold before a scheduled trip. Triggers passenger notification.
- **LoyaltyTierChanged**: A passenger's loyalty tier has changed. Triggers benefit recalculation on future bookings.

### Itinerary Context Events

- **ItineraryCreated**: A new itinerary has been assembled from confirmed reservation segments.
- **ItineraryUpdated**: The itinerary has been modified due to segment changes, additions, or removals.
- **ConnectionAtRisk**: A schedule change has reduced connection time below the minimum. Triggers rebooking evaluation.

### Payment & Billing Context Events

- **PaymentAuthorized**: A payment method has been authorized for a specified amount. Triggers reservation confirmation.
- **PaymentCaptured**: Funds have been settled. Triggers ticket issuance and BSP reporting.
- **PaymentFailed**: Authorization or capture failed. Triggers reservation hold and passenger notification.
- **RefundProcessed**: A refund has been issued to the passenger. Triggers accounting update and notification.
- **InvoiceGenerated**: An invoice has been created for the booking. Triggers delivery to traveler or agent.

### Search & Availability Context Events

- **SearchPerformed**: A search query has been executed. Used for analytics and personalization, not for transactional coordination.
- **AvailabilityChanged**: Cached availability data has been invalidated. Triggers cache refresh.

## Event Design Principles

1. **Past tense naming**: Events describe what happened, not what should happen. `ReservationConfirmed`, not `ConfirmReservation`.
2. **Self-contained data**: Events carry all data needed for consumers to react without querying the producer.
3. **Immutability**: Events are facts about the past and never change once published.
4. **Ordering guarantees**: Events from the same aggregate are delivered in order. Events across aggregates may arrive out of order.
5. **Idempotent consumers**: Consumers must handle receiving the same event multiple times without adverse effects.

## Event Flow Example

1. Passenger selects flights and submits booking
2. Reservation Context publishes `ReservationCreated`
3. Pricing & Fare Context receives event, validates fare, publishes `FareQuoted`
4. Payment & Billing Context receives confirmation, authorizes payment, publishes `PaymentAuthorized`
5. Reservation Context receives authorization, confirms with GDS, publishes `ReservationConfirmed`
6. Payment & Billing Context captures payment, publishes `PaymentCaptured`
7. Reservation Context issues tickets, publishes `ReservationTicketed`
8. Itinerary Context receives ticketed event, assembles itinerary, publishes `ItineraryCreated`

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Aggregates produce domain events as side effects of state changes
- [Event-Driven Architecture](../event-driven-architecture/) -- The architectural pattern enabling event-based communication
- [Bounded Contexts](../bounded-contexts/) -- Events flow between bounded contexts
- [Repositories](../repositories/) -- Event-sourced repositories store events as the source of truth
- [Integration Patterns](../integration-patterns/) -- Events are the primary integration mechanism

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 11: Domain Events. O'Reilly, 2021.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
