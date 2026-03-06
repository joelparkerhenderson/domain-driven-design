# Pricing & Fare Context

## Overview

The Pricing & Fare Context handles all aspects of travel pricing: fare calculation, fare rule interpretation, tax computation, discount application, ancillary pricing, and revenue management decisions. This context translates complex airline tariff data into actionable pricing that other contexts can consume. It must handle fare combinations, round-trip and one-way pricing, multi-city fare construction, fare filing validation, and the application of negotiated corporate or agency fares. The domain is intensely rule-driven and deeply dependent on external fare data sources.

## Key Domain Concepts

### Fare

The base price for air transportation between two points, excluding taxes and surcharges. A fare is defined by its origin, destination, fare basis code, fare class, directionality, carrier, and validity period. Fares are filed by airlines with ATPCO (Airline Tariff Publishing Company) and distributed to GDS platforms.

### Fare Basis Code

An alphanumeric code that encodes the fare class, restrictions, route type, season, and other attributes. For example, "YOWUS" might represent full-fare one-way US domestic economy. The fare basis code is the key to looking up fare rules. Modeled as a value object.

### Fare Rule

A set of conditions that govern a fare. ATPCO organizes fare rules into categories:

- **Category 5**: Advance Purchase -- how far in advance the ticket must be purchased
- **Category 6**: Minimum Stay -- minimum time at the destination before return
- **Category 7**: Maximum Stay -- maximum time permitted at the destination
- **Category 10**: Combinability -- which other fares this fare can be combined with
- **Category 16**: Penalties -- fees for changes and cancellations
- **Category 33**: Voluntary Changes -- rules for ticket exchanges

### Fare Construction

The process of combining individual fares to price a multi-segment itinerary. Methods include:

- **Published fares**: Direct origin-to-destination fares filed by the airline
- **Constructed fares**: Fares built by combining published fares using add-on amounts
- **Fare-by-rule**: Fares derived from a base fare by applying percentage or amount adjustments

### Tax Breakdown

Itemized government and airport taxes applied to a booking. Each tax has a code, rate (fixed or percentage), and jurisdiction. Taxes vary by origin, destination, routing, and passenger nationality. Examples: US transportation excise tax (US), passenger facility charge (XF), UK Air Passenger Duty (GB), German aviation tax (DE).

### FareQuote (Aggregate Root)

The aggregate representing a complete priced itinerary. Contains fare components (one per pricing unit), tax breakdowns, total amounts, currency, and validity period. The FareQuote enforces that the total equals the sum of components plus taxes and that all components use consistent currency.

## Revenue Management Integration

The Pricing & Fare Context interfaces with revenue management decisions that control which booking classes are open for sale and at what fare levels. Revenue management is typically a separate system, but its outputs (booking class availability, bid prices) are consumed by this context to determine which fares are available for a given search.

## Invariants

- Total fare must equal the sum of all fare components plus all applicable taxes
- Each fare component must reference a valid, non-expired fare basis code
- Fare quotes expire after a configured validity period (typically 15-30 minutes)
- Currency must be consistent within a single fare quote
- Fare construction must comply with ATPCO combinability rules (Category 10)
- Corporate or negotiated fares require valid account codes and may have volume commitments

## Integration Points

- **ATPCO**: Upstream fare data supplier providing tariff filings, fare rules, and construction tables
- **Search & Availability Context**: Consumes fare quotes for display pricing
- **Reservation Context**: Provides confirmed pricing for bookings
- **Payment & Billing Context**: Provides amounts and tax breakdowns for invoicing
- **Anti-Corruption Layer**: ATPCO data formats and GDS fare responses are translated through ACLs

## Domain Events

- **FareQuoted**: A fare has been calculated and is available for booking
- **FareExpired**: A quoted fare's validity period has elapsed
- **FareRuleViolated**: A booking modification would violate fare rules
- **TaxRateUpdated**: A tax rate has changed, affecting future fare calculations

## Relationships to Other Topics

- [Value Objects](../value-objects/) -- Fares, fare basis codes, currencies, and taxes are value objects
- [Aggregates](../aggregates/) -- FareQuote is the primary aggregate root
- [Domain Services](../domain-services/) -- FareCalculationService orchestrates fare computation
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ATPCO and GDS fare data translated through ACLs
- [Reservation Context](../reservation-context/) -- Consumes fare quotes for booking pricing
- [Travel Standards](../travel-standards/) -- ATPCO and IATA standards govern fare structures

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- IATA. "Resolution 024d: Standard Fare and Rule Filing Procedures."
- ATPCO. "ATPCO Fare Filing and Distribution Documentation."
- Doganis, Rigas. "Flying Off Course: Airline Economics and Marketing." Routledge, 2019.
