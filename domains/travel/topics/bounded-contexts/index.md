# Bounded Contexts in Travel

## Overview

A bounded context is a logical boundary within which a particular domain model is defined and applicable. Inside a bounded context, terms have precise, unambiguous meanings, and the model is internally consistent. Different bounded contexts may use the same term (e.g., "booking") with different meanings, and that is expected and correct. In travel, bounded contexts separate the concerns of searching for availability, managing reservations, calculating fares, handling passengers, assembling itineraries, and processing payments.

## Relevance to Travel

Travel systems involve many stakeholders with different perspectives. A "reservation" in the search context is a tentative hold on inventory, while in the reservation context it is a confirmed booking with passenger details and payment. A "fare" in the pricing context is a complex object with rules, restrictions, and tax breakdowns, while in the itinerary context it may be a simple display amount. Bounded contexts prevent these different perspectives from corrupting each other.

## Travel Bounded Contexts

### Search & Availability Context

Responsible for querying multiple GDS platforms and direct airline/hotel connections for real-time availability. This context deals with volatile, short-lived data: seat counts change by the second, hotel rooms sell out, and cached results expire quickly. The model here emphasizes search criteria, availability responses, and caching strategies rather than booking permanence.

### Reservation Context

The core transactional context where bookings are created, modified, and cancelled. The Reservation is the aggregate root, encompassing flight segments, hotel stays, car rentals, and ancillary services. This context owns the PNR lifecycle, ensures booking consistency, and coordinates with external systems for confirmation. It emphasizes data integrity and state transitions.

### Pricing & Fare Context

Handles fare calculation, fare rule interpretation, tax computation, discount application, and revenue management decisions. This context translates complex airline tariff data into actionable pricing. It must handle fare combinations, round-trip vs. one-way pricing, multi-city fare construction, and ancillary pricing independently from the reservation lifecycle.

### Passenger Management Context

Manages passenger identities, travel documents (passports, visas), contact information, frequent flyer memberships, preferences (meal, seat), and special service requests. This context treats the passenger as a long-lived profile that exists across many reservations, distinct from the per-booking passenger record in the reservation context.

### Itinerary Context

Assembles travel segments into coherent trip plans, validates connections against minimum connection times, checks schedule feasibility, and presents the traveler's journey as a unified document. This context consumes data from Search, Reservation, and Pricing contexts but maintains its own model of what constitutes a valid, presentable itinerary.

### Payment & Billing Context

Processes payments, manages payment methods, handles authorization and capture, produces invoices, processes refunds, and reconciles with IATA BSP settlement. This context enforces PCI DSS compliance and treats financial data with its own security and audit requirements independent of the booking model.

## Context Boundaries in Practice

Each bounded context:

- Has its own ubiquitous language (e.g., "segment" means a flight leg in Reservation but a display block in Itinerary)
- Owns its own data store and does not share database tables with other contexts
- Communicates with other contexts through well-defined interfaces: domain events, APIs, or anti-corruption layers
- Can be developed, deployed, and scaled independently
- Has its own team ownership and release cadence

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Bounded contexts emerge from strategic design decisions
- [Context Map](../context-map/) -- Documents relationships between bounded contexts
- [Ubiquitous Language](../ubiquitous-language/) -- Each bounded context defines its own precise vocabulary
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects bounded contexts from external system models
- [Aggregates](../aggregates/) -- Each bounded context defines its own aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapters 5-6. O'Reilly, 2021.
