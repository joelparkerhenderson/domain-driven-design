# Event-Driven Architecture in Travel

## Overview

Event-driven architecture (EDA) is a design pattern where bounded contexts communicate through the production, detection, and consumption of domain events. Rather than contexts calling each other directly through synchronous APIs, they publish events when significant state changes occur, and interested contexts subscribe to and react to those events independently. In travel, EDA enables the complex, multi-step booking lifecycle to unfold across independent contexts without tight coupling.

## Relevance to Travel

The travel booking lifecycle involves a chain of operations across multiple contexts: search leads to reservation, reservation triggers pricing validation, pricing confirmation triggers payment, payment success triggers ticketing, ticketing triggers itinerary generation, and itinerary creation triggers notification. Without EDA, this chain would require each context to know about and call the next, creating a fragile monolith. EDA allows each context to evolve independently while maintaining the overall workflow through events.

## Event Flows

### Booking Creation Flow

1. **OfferSelected** (Search & Availability) -- Traveler selects a flight/hotel combination
2. **ReservationCreated** (Reservation) -- Booking created with segments and passengers
3. **FareQuoted** (Pricing & Fare) -- Fare validated and quoted for the booking
4. **PaymentAuthorized** (Payment & Billing) -- Payment method authorized
5. **ReservationConfirmed** (Reservation) -- Segments confirmed with GDS
6. **PaymentCaptured** (Payment & Billing) -- Funds settled
7. **ReservationTicketed** (Reservation) -- Tickets issued
8. **ItineraryCreated** (Itinerary) -- Trip document assembled and delivered

### Schedule Change Flow

1. **SegmentChanged** (Reservation) -- Airline schedule change received via GDS
2. **ItineraryUpdated** (Itinerary) -- Itinerary reflects new times
3. **ConnectionAtRisk** (Itinerary) -- If connection time is now below MCT
4. Notification sent to passenger with options (accept, rebook, cancel)

### Cancellation Flow

1. **ReservationCancelled** (Reservation) -- Booking cancelled by passenger or agent
2. **RefundProcessed** (Payment & Billing) -- Refund calculated and issued per fare rules
3. **ItineraryUpdated** (Itinerary) -- Itinerary removed or segments marked cancelled
4. Loyalty points reversal triggered in external loyalty system

### Document Expiry Flow

1. **DocumentExpiring** (Passenger Management) -- Passport expiring before travel date
2. Notification sent to passenger to renew document
3. If document not updated before departure, booking may require attention

## Event Characteristics

### Eventual Consistency

Events are processed asynchronously. The Itinerary Context may learn about a confirmed reservation milliseconds or seconds after the event is published, not at the exact moment of confirmation. The system is designed to tolerate this lag.

### Idempotent Processing

Consumers must handle receiving the same event multiple times without adverse effects. A duplicate `PaymentAuthorized` event should not result in double capture.

### Event Ordering

Events from the same aggregate are guaranteed to arrive in order. Events from different aggregates may arrive in any order. Consumers must handle this -- for example, an `ItineraryUpdated` event might arrive before the consumer has processed the `ItineraryCreated` event.

### Event Versioning

As the domain model evolves, event schemas change. Events carry a version number, and consumers must handle multiple versions during transition periods.

## Saga / Process Manager Pattern

Long-running booking workflows that span multiple contexts are coordinated by saga or process manager patterns:

- **Booking Saga**: Coordinates the creation flow, ensuring that if payment fails, the reservation is released and the GDS hold is cancelled
- **Refund Saga**: Coordinates cancellation, ensuring that refund is calculated, payment is refunded, and itinerary is updated
- **Schedule Change Saga**: Coordinates the response to airline schedule changes across reservation, itinerary, and notification concerns

## Infrastructure Considerations

- **Message broker**: Events are published to a message broker (e.g., Apache Kafka, RabbitMQ, AWS SNS/SQS) for reliable delivery
- **Dead letter queues**: Failed event processing is routed to dead letter queues for investigation
- **Event store**: Events may be persisted for audit, replay, and event sourcing
- **Monitoring**: Event flow monitoring detects stuck sagas, unprocessed events, and consumer lag

## Relationships to Other Topics

- [Domain Events](../domain-events/) -- The domain events that flow through the architecture
- [Bounded Contexts](../bounded-contexts/) -- EDA enables loose coupling between contexts
- [Integration Patterns](../integration-patterns/) -- EDA is a foundational integration pattern
- [Aggregates](../aggregates/) -- Aggregates produce the events that drive the architecture
- [Context Map](../context-map/) -- The context map shows which events flow between which contexts

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapters 8, 13. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 15: Event-Driven Architecture. O'Reilly, 2021.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Richardson, Chris. "Microservices Patterns." Manning, 2018.
