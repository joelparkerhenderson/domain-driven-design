# Ubiquitous Language in Travel

## Overview

Ubiquitous language is the shared vocabulary used by developers, domain experts, and stakeholders within a bounded context. Every term has a precise, agreed-upon definition that appears consistently in conversations, documentation, and code. The ubiquitous language eliminates ambiguity by ensuring that when a revenue manager says "fare class" and a developer writes `FareClass`, they mean exactly the same thing. In travel, where industry jargon is dense and overlapping, establishing ubiquitous language is essential.

## Relevance to Travel

The travel industry has decades of established terminology from IATA, airlines, GDS platforms, and hotel chains. Many terms carry multiple meanings across contexts: a "booking" can mean a search session, a tentative hold, or a confirmed reservation. "Class" can refer to a physical cabin (Economy, Business) or a booking class letter (Y, B, M). Without ubiquitous language, these ambiguities propagate into code, creating bugs and miscommunication.

## Language by Bounded Context

### Search & Availability Context

- **Search Request**: A query specifying origin, destination, dates, passenger count, and cabin preference
- **Availability Response**: A set of options returned by GDS or airline, each with flight details, booking classes, and seat counts
- **Offer**: A priced, bookable combination of segments presented to the user
- **Cache TTL**: The time-to-live for cached availability data before it must be refreshed from the source

### Reservation Context

- **Reservation**: A confirmed booking encompassing one or more segments, passengers, and ancillary services
- **PNR**: The Passenger Name Record identifier assigned by the GDS or airline
- **Segment**: A single flight, hotel stay, or car rental within a reservation
- **Ticketing Deadline**: The date by which a reservation must be ticketed or it will be automatically cancelled
- **Void**: Cancellation of a ticket within the permitted void window (typically 24 hours)

### Pricing & Fare Context

- **Fare**: The base price for air transportation, excluding taxes and surcharges
- **Fare Basis Code**: An alphanumeric code encoding fare class, restrictions, season, and routing
- **Fare Rule**: Conditions governing a fare including advance purchase, minimum stay, change fees, and refund eligibility
- **Tax Breakdown**: Itemized government and airport taxes applied to a fare
- **Fare Construction**: The process of combining fares for multi-segment itineraries

### Passenger Management Context

- **Passenger Profile**: A persistent record of a traveler's identity, documents, preferences, and loyalty information
- **Travel Document**: A passport, visa, or national ID associated with a passenger
- **Frequent Flyer Number**: A loyalty program identifier linked to a passenger profile
- **Special Service Request (SSR)**: A coded request for passenger-specific services such as wheelchair assistance or dietary needs

### Itinerary Context

- **Itinerary**: A chronologically ordered collection of segments comprising a trip
- **Connection**: A transfer between segments at an intermediate point
- **Minimum Connection Time (MCT)**: The minimum time required to transfer between flights at a given airport
- **Stopover**: A deliberate break in the journey exceeding 24 hours (international) or 4 hours (domestic)

### Payment & Billing Context

- **Payment Authorization**: A hold placed on a payment method confirming funds availability
- **Capture**: The settlement of an authorized payment amount
- **Refund**: Return of funds to the original payment method for unused services
- **Invoice**: A financial document detailing charges, taxes, and payment for a booking
- **BSP Remittance**: Financial settlement between travel agent and airlines through IATA BSP

## Language Governance

- Maintain a glossary (see [language.md](../../language.md)) as the authoritative reference
- Review and update terms during domain modeling sessions with travel experts
- Ensure code identifiers (class names, method names, variable names) mirror the ubiquitous language exactly
- Flag and resolve conflicts when the same term carries different meanings across contexts

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Each bounded context defines its own ubiquitous language
- [Strategic Design](../strategic-design/) -- Language emerges from knowledge crunching in strategic design
- [Context Map](../context-map/) -- Translation between languages occurs at context boundaries
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate external terminology into the ubiquitous language
- [Travel Standards](../travel-standards/) -- IATA and GDS standards inform the language but do not dictate it

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 2: Communication and the Use of Language. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 1: Getting Started with DDD. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 2: Discovering Domain Knowledge. O'Reilly, 2021.
