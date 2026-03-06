# Anti-Corruption Layer in Travel

## Overview

An Anti-Corruption Layer (ACL) is a translation boundary that prevents external system models, data formats, and terminology from leaking into a bounded context's internal domain model. The ACL translates between the external system's representation and the internal ubiquitous language, protecting the purity and consistency of the domain model. In travel, ACLs are critical because the system integrates with multiple legacy GDS platforms, airline reservation systems, and payment processors, each with its own data formats and conventions.

## Relevance to Travel

Travel systems depend heavily on external Global Distribution Systems (Amadeus, Sabre, Travelport) that expose decades-old data formats, cryptic codes, and proprietary message structures. Without ACLs, these external representations would contaminate the internal model, making it difficult to reason about, test, and evolve. The ACL ensures that internally a `FareClass` is a well-defined value object, even though externally it arrives as a single character embedded in a pipe-delimited string.

## GDS Anti-Corruption Layers

### Amadeus ACL

Amadeus uses a SOAP-based API with complex XML schemas. The ACL translates:

- Amadeus XML availability responses into internal `AvailabilityResponse` objects
- Amadeus PNR structures (with cryptic field codes) into internal `Reservation` aggregates
- Amadeus fare quote responses into internal `FareQuote` value objects
- Amadeus ticket image data into internal `Ticket` entities

### Sabre ACL

Sabre uses a mix of SOAP and REST APIs with its own message formats. The ACL translates:

- Sabre `BargainFinderMaxRQ` responses into internal search result models
- Sabre PNR locator records into internal `Reservation` aggregates
- Sabre `EnhancedAirBookRQ` responses into booking confirmations
- Sabre queue management messages into internal workflow events

### Travelport ACL

Travelport (Galileo/Apollo/Worldspan) uses its Universal API. The ACL translates:

- Travelport availability and pricing responses into internal models
- Travelport Universal Record formats into internal `Reservation` aggregates
- Provider-specific booking codes into internal `BookingClass` value objects

## Other ACL Boundaries

### Payment Processor ACL

Translates between payment gateway APIs (Stripe, Adyen, Worldpay) and the internal `Payment` domain model:

- External transaction IDs to internal `PaymentReference` value objects
- Gateway-specific status codes to internal `PaymentStatus` enumerations
- Tokenized card data to internal `PaymentMethod` value objects

### Airline Direct Connect ACL

Translates airline-specific NDC API responses into the internal model:

- Airline offer structures into internal `Offer` objects
- Airline order responses into internal `Reservation` segments
- Airline-specific ancillary catalogs into internal `AncillaryService` objects

### ATPCO Fare Data ACL

Translates ATPCO tariff filing formats into the internal pricing model:

- Category rules (Cat 5 advance purchase, Cat 6 minimum stay) into internal `FareRule` objects
- Fare construction tables into internal routing and combination rules

## ACL Design Principles

1. **Isolate translation logic**: ACL code lives in its own module, separate from domain logic and external client libraries
2. **Map to domain concepts**: Every external data element maps to a domain object -- never pass raw GDS data through the domain layer
3. **Handle format variations**: Different GDS platforms return different formats for the same concept; the ACL normalizes these into a single internal representation
4. **Fail explicitly**: When external data cannot be translated, the ACL raises domain-meaningful errors rather than passing corrupt data
5. **Version independently**: ACLs can be updated when external APIs change without modifying the internal domain model

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- ACLs sit at the boundaries of bounded contexts
- [Context Map](../context-map/) -- ACLs appear as translation layers on the context map
- [Integration Patterns](../integration-patterns/) -- ACLs are a key integration pattern
- [Travel Standards](../travel-standards/) -- ACLs translate between GDS-specific and IATA-standard formats
- [Value Objects](../value-objects/) -- ACLs convert external data into well-typed value objects

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 8: Anti-Corruption Layer. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
