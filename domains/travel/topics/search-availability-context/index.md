# Search & Availability Context

## Overview

The Search & Availability Context is responsible for querying multiple travel providers -- GDS platforms (Amadeus, Sabre, Travelport), airline direct connections (NDC), hotel aggregators, and car rental systems -- to find available flights, hotel rooms, and rental vehicles matching a traveler's criteria. This context deals with highly volatile, short-lived data: seat availability changes by the second, hotel rooms sell out unpredictably, and cached results expire quickly. The model emphasizes search request construction, multi-source aggregation, caching strategies, and result ranking.

## Key Domain Concepts

### Search Request

A structured query capturing the traveler's intent: origin, destination, travel dates, passenger count and types (adult, child, infant), cabin class preference, and optional constraints (direct flights only, preferred airlines, maximum stops). The search request is a value object -- it is immutable once submitted and defines the scope of the availability query.

### Availability Response

The aggregated results from one or more providers, normalized into a consistent internal format. Each result contains flight options (or hotel options, or car options) with pricing, booking class availability, and relevant restrictions. The ACL translates each provider's response format into this unified model.

### Offer

A bookable combination of segments with associated pricing. An offer represents a complete, purchasable product presented to the traveler. Offers have a short validity period (minutes, not hours) because the underlying availability is volatile.

### Availability Cache

A time-limited store of recent availability data. The cache reduces GDS query volume (which has cost and rate-limit implications) but must balance freshness against staleness. Cache TTL varies by product type: flights (30-120 seconds), hotels (5-15 minutes), car rentals (15-60 minutes).

## Aggregates

This context does not have traditional long-lived aggregates. Search requests and responses are transient. The closest aggregate-like concept is a **SearchSession**, which tracks a user's search activity for analytics and personalization but is not a transactional consistency boundary.

## Integration Points

- **GDS platforms**: Amadeus, Sabre, and Travelport via Anti-Corruption Layers translating XML/SOAP responses
- **Airline NDC connections**: Direct airline APIs using IATA NDC standard for richer content and ancillaries
- **Hotel aggregators**: Aggregation platforms providing normalized hotel availability
- **Car rental systems**: Direct connections to car rental inventory systems
- **Pricing & Fare Context**: Consulted for fare details when GDS responses include fare data
- **Passenger Management Context**: Passenger preferences influence search ranking and filtering

## Domain Events

- **SearchPerformed**: Emitted for analytics when a search is executed (not a transactional event)
- **AvailabilityChanged**: Emitted when cached data is invalidated or refreshed
- **OfferSelected**: Emitted when a traveler selects an offer, triggering the Reservation Context

## Challenges

- **Volatility**: Availability can change between search and booking, requiring "look-to-book" validation
- **GDS costs**: Each GDS query has a transaction cost; caching and deduplication are essential
- **Response normalization**: Different providers return data in different structures and with different semantics
- **Performance**: Travelers expect sub-second search results, requiring parallel provider queries and efficient caching
- **Rate limiting**: GDS and airline APIs impose rate limits that must be managed across the platform

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate provider-specific formats into the internal model
- [Reservation Context](../reservation-context/) -- Selected offers flow to the Reservation Context for booking
- [Pricing & Fare Context](../pricing-fare-context/) -- Fare data is sourced from or validated against this context
- [Integration Patterns](../integration-patterns/) -- Multi-provider search uses specific integration strategies
- [Travel Standards](../travel-standards/) -- GDS protocols and IATA NDC define the external interfaces

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- IATA. "New Distribution Capability (NDC) Standard." Version 21.3.
- Amadeus. "Amadeus Web Services Documentation."
- Sabre. "Sabre Dev Studio REST/SOAP API Reference."
