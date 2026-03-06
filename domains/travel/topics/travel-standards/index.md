# Travel Standards

## Overview

The travel industry operates on a foundation of established standards that govern distribution, messaging, ticketing, settlement, and data exchange. These standards, maintained primarily by IATA (International Air Transport Association) and implemented through GDS platforms, define the published languages and data formats that travel systems must understand. In a DDD context, these standards inform value object design, shape Anti-Corruption Layer translations, and define the published language used in inter-system communication.

## Global Distribution Systems (GDS)

### Amadeus

Amadeus is the world's largest GDS by market share, headquartered in Madrid. It serves airlines, hotels, car rental companies, and travel agencies.

- **API**: Amadeus Web Services (SOAP/XML) and Amadeus for Developers (REST/JSON)
- **PNR format**: Amadeus PNR with record locator (6-character alphanumeric)
- **Booking flow**: Air Availability, Air Sell, PNR creation, Fare Quote, Ticketing
- **Coverage**: Strong in Europe, Middle East, Africa; global airline content
- **DDD relevance**: ACL translates Amadeus XML schemas into internal domain objects

### Sabre

Sabre is a major GDS headquartered in Southlake, Texas. Originally developed by American Airlines.

- **API**: Sabre Dev Studio (SOAP and REST), BargainFinderMax for shopping
- **PNR format**: Sabre PNR with record locator (6-character alphanumeric)
- **Booking flow**: BargainFinderMaxRQ (search), EnhancedAirBookRQ (book), PassengerDetailsRQ (add passengers)
- **Coverage**: Strong in North America and Latin America
- **DDD relevance**: ACL translates Sabre-specific response structures into internal models

### Travelport

Travelport operates the Galileo, Apollo, and Worldspan GDS brands through a unified Universal API.

- **API**: Travelport Universal API (SOAP/XML)
- **PNR format**: Universal Record with provider-specific locators
- **Booking flow**: Low Fare Search, Air Create Reservation, Air Price, Air Ticketing
- **Coverage**: Galileo (Europe/Asia), Apollo (North America), Worldspan (global)
- **DDD relevance**: ACL normalizes responses from different Travelport providers into a single internal model

## IATA Standards

### NDC (New Distribution Capability)

IATA NDC is a standard (based on XML/JSON schemas) enabling airlines to distribute rich content and ancillaries through a modern API, bypassing traditional GDS limitations.

- **Purpose**: Allow airlines to differentiate their products with branded fares, bundled offers, rich media, and dynamic pricing
- **Messages**: OfferRequest/OfferResponse, OrderCreate, OrderRetrieve, OrderChange, OrderCancel
- **Versions**: NDC 17.2, 18.1, 19.1, 21.3 -- each adds capabilities
- **DDD relevance**: NDC messages serve as a published language between airlines and distributors; ACLs translate NDC responses into internal domain objects

### BSP (Billing and Settlement Plan)

IATA BSP is the financial settlement system between travel agents and airlines.

- **Purpose**: Centralize and simplify financial transactions, reducing bilateral settlement between thousands of agents and airlines
- **Cycle**: Bi-weekly billing periods with defined reporting and remittance dates
- **Reports**: BSP HOT (Hand-Off Transaction) records, sales reports, refund reports
- **DDD relevance**: The Payment & Billing Context generates BSP-compliant transaction records; BSP formats define the published language for financial settlement

### Airline and Airport Codes

- **IATA airline designator**: Two-letter carrier codes (e.g., UA = United Airlines, BA = British Airways, LH = Lufthansa)
- **ICAO airline designator**: Three-letter codes used in flight operations (e.g., UAL, BAW, DLH)
- **IATA airport code**: Three-letter location codes (e.g., JFK, LHR, NRT, LAX)
- **IATA city code**: Three-letter city codes that may group multiple airports (e.g., NYC for JFK/LGA/EWR)
- **DDD relevance**: Modeled as value objects (AirlineCode, AirportCode) with validation against IATA reference data

### AIRIMP (ATA/IATA Reservations Interline Message Procedures)

AIRIMP defines the standard message formats for inter-airline communication about reservations and ticketing.

- **Purpose**: Enable airlines to exchange reservation data for interlining, codeshare, and schedule changes
- **Messages**: Booking request/reply, ticket notification, schedule change notification, waitlist clearance
- **Format**: Structured text messages with defined field positions and codes
- **DDD relevance**: AIRIMP messages are the published language for inter-airline reservation exchange; ACLs parse AIRIMP messages into internal domain events

## PNR Format

The Passenger Name Record is the industry-standard data structure for a travel reservation.

- **Record locator**: 6-character alphanumeric identifier (e.g., ABC123)
- **Standard fields**: Passenger names, itinerary segments, ticketing information, contact details, remarks, SSR codes
- **Variants**: Each GDS and airline has its own PNR format variation, though the core fields are consistent
- **DDD relevance**: The Reservation aggregate maps to a PNR; the ACL translates between external PNR formats and the internal Reservation model

## PCI DSS (Payment Card Industry Data Security Standard)

PCI DSS governs the handling of credit card data in payment processing.

- **Version**: PCI DSS v4.0 (effective March 2024)
- **Requirements**: 12 high-level requirements covering network security, data protection, access control, monitoring, and testing
- **Scope**: Applies to any system that stores, processes, or transmits cardholder data
- **Key rules**: Never store full card numbers in the domain; use tokenization; encrypt transmission; restrict access
- **DDD relevance**: The Payment & Billing Context uses tokenized payment references (value objects) rather than raw card data; PCI compliance shapes the architecture of the payment bounded context

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate between GDS/IATA formats and the internal model
- [Value Objects](../value-objects/) -- IATA codes and industry data types are modeled as value objects
- [Integration Patterns](../integration-patterns/) -- Standards define the published languages used in integrations
- [Ubiquitous Language](../ubiquitous-language/) -- Industry standards inform but do not dictate the ubiquitous language
- [Payment & Billing Context](../payment-billing-context/) -- PCI DSS and BSP standards govern this context
- [Regulatory Compliance](../regulatory-compliance/) -- PCI DSS is both a standard and a compliance requirement

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- IATA. "New Distribution Capability (NDC) Standard Documentation."
- IATA. "AIRIMP: ATA/IATA Reservations Interline Message Procedures." 31st Edition.
- IATA. "BSP Manual for Agents."
- IATA. "Airline Coding Directory."
- PCI Security Standards Council. "PCI DSS v4.0."
- Amadeus. "Amadeus Web Services Technical Documentation."
- Sabre. "Sabre Dev Studio API Documentation."
- Travelport. "Travelport Universal API Documentation."
