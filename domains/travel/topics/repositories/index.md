# Repositories in Travel

## Overview

A repository is an abstraction that encapsulates the logic for retrieving and persisting aggregate roots. It provides a collection-like interface to the domain layer, hiding the details of data storage and retrieval. The domain model interacts with repositories using domain concepts (e.g., "find reservation by PNR") rather than database queries. Each aggregate root has exactly one repository. In travel, repositories manage the persistence of reservations, fare quotes, passenger profiles, itineraries, and payments.

## Relevance to Travel

Travel systems must store and retrieve complex aggregate roots that have intricate internal structures. A Reservation contains segments, passenger records, and ancillary services. A FareQuote contains fare components, tax breakdowns, and validity periods. Repositories provide a clean boundary between the domain model and the persistence mechanism, allowing the domain to remain pure and independent of database technology choices.

## Travel Repositories

### ReservationRepository

The most critical repository in the travel domain. Provides access to Reservation aggregate roots.

- **Find by ReservationId**: Retrieve a reservation by its internal identifier
- **Find by PNR**: Retrieve a reservation by its GDS-assigned Passenger Name Record locator
- **Find by Passenger**: Retrieve all reservations associated with a specific passenger
- **Find upcoming by Passenger**: Retrieve reservations with future travel dates for a passenger
- **Find by ticketing deadline**: Retrieve reservations approaching their ticketing deadline for automated processing
- **Save**: Persist a new or modified reservation with all its internal entities and value objects

### FareQuoteRepository

Manages persistence of FareQuote aggregate roots.

- **Find by FareQuoteId**: Retrieve a specific fare quote
- **Find by ReservationId**: Retrieve fare quotes associated with a reservation
- **Find active quotes**: Retrieve non-expired fare quotes for re-pricing or display
- **Save**: Persist a new fare quote with its fare components, taxes, and rules

### PassengerProfileRepository

Manages persistence of PassengerProfile aggregate roots.

- **Find by PassengerProfileId**: Retrieve a passenger profile by its internal identifier
- **Find by travel document**: Retrieve a profile by passport number and issuing country
- **Find by loyalty number**: Retrieve a profile by frequent flyer program and membership number
- **Find by name and date of birth**: Retrieve potential profile matches for deduplication
- **Save**: Persist a new or modified passenger profile

### ItineraryRepository

Manages persistence of Itinerary aggregate roots.

- **Find by ItineraryId**: Retrieve a specific itinerary
- **Find by ReservationId**: Retrieve the itinerary associated with a reservation
- **Find active by Passenger**: Retrieve current and upcoming itineraries for a passenger
- **Save**: Persist a new or modified itinerary with its segments

### PaymentRepository

Manages persistence of Payment aggregate roots.

- **Find by PaymentId**: Retrieve a specific payment record
- **Find by ReservationId**: Retrieve all payments associated with a reservation
- **Find pending authorizations**: Retrieve authorized but uncaptured payments for settlement processing
- **Find by payment reference**: Retrieve a payment by its external processor reference
- **Save**: Persist a new or modified payment record

## Repository Design Principles

1. **One repository per aggregate root**: There is a ReservationRepository but not a SegmentRepository. Segments are accessed through their parent Reservation aggregate.

2. **Domain-oriented interface**: Repository methods use domain concepts. `findByPNR("ABC123")` rather than `SELECT * FROM reservations WHERE pnr = 'ABC123'`.

3. **Return complete aggregates**: A repository always returns the full aggregate with all its internal entities and value objects loaded. Partial loading creates consistency risks.

4. **Encapsulate query logic**: Complex queries (e.g., finding reservations with expiring ticketing deadlines) belong in the repository, not in domain services or application code.

5. **Infrastructure independence**: The repository interface is defined in the domain layer. The implementation (SQL, document store, event store) lives in the infrastructure layer.

## Persistence Strategies

### State-Based Persistence

The traditional approach where the current state of the aggregate is stored directly. Suitable for passenger profiles and itineraries where the current state is the primary concern.

### Event-Sourced Persistence

An alternative where the repository stores the sequence of domain events that produced the aggregate's current state. Particularly valuable for reservations and payments where the complete history of changes (who changed what, when, and why) has business and regulatory significance.

### Caching Considerations

Travel-specific caching strategies include:

- **Availability cache**: Short TTL (seconds to minutes) for volatile GDS availability data
- **Fare cache**: Medium TTL (minutes to hours) for fare quotes that change less frequently
- **Profile cache**: Long TTL for passenger profiles that change infrequently
- **PNR cache**: Invalidated on any modification, as PNR data must be authoritative

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Repositories persist and retrieve aggregate roots
- [Entities](../entities/) -- Aggregate roots are entities managed by repositories
- [Domain Events](../domain-events/) -- Event-sourced repositories store domain events
- [Domain Services](../domain-services/) -- Domain services use repositories to access aggregates
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context has its own repositories

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12: Repositories. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 12: Repositories. O'Reilly, 2021.
