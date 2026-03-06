# Travel - Topics

## Strategic Design

- [Strategic Design](strategic-design/) -- Strategic DDD patterns applied to travel
- [Bounded Contexts](bounded-contexts/) -- Travel bounded context definitions
- [Context Map](context-map/) -- Relationships between travel contexts
- [Ubiquitous Language](ubiquitous-language/) -- Shared travel terminology
- [Subdomains](subdomains/) -- Core, supporting, and generic travel subdomains
- [Anti-Corruption Layer](anti-corruption-layer/) -- Translation boundaries with GDS and airline systems

## Tactical Design

- [Aggregates](aggregates/) -- Travel aggregate roots and consistency boundaries
- [Entities](entities/) -- Identifiable travel domain objects
- [Value Objects](value-objects/) -- Immutable travel descriptive objects
- [Domain Events](domain-events/) -- Significant travel occurrences
- [Repositories](repositories/) -- Persistence abstractions for travel aggregates
- [Domain Services](domain-services/) -- Cross-aggregate travel operations

## Bounded Contexts

- [Search & Availability Context](search-availability-context/) -- Flight, hotel, and car rental search
- [Reservation Context](reservation-context/) -- Booking management and lifecycle
- [Pricing & Fare Context](pricing-fare-context/) -- Fare calculation and pricing rules
- [Passenger Management Context](passenger-management-context/) -- Passenger profiles and documents
- [Itinerary Context](itinerary-context/) -- Trip planning and segment management
- [Payment & Billing Context](payment-billing-context/) -- Payment processing and settlement

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) -- Event flows between travel contexts
- [Integration Patterns](integration-patterns/) -- Integration with GDS, airlines, and payment processors
- [Travel Standards](travel-standards/) -- Industry standards: IATA, NDC, GDS protocols, PNR
- [Regulatory Compliance](regulatory-compliance/) -- GDPR, PCI DSS, EU passenger rights, security regulations
