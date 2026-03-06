# Domain Services in Travel

## Overview

A domain service is a stateless operation that represents domain logic which does not naturally belong to any single entity or value object. Domain services encapsulate business rules that span multiple aggregates, require coordination between entities, or represent a domain concept that is a verb rather than a noun. In travel, domain services handle complex operations like fare calculation, availability search orchestration, booking validation, and connection feasibility analysis.

## Relevance to Travel

Travel domain logic frequently involves coordination across multiple aggregates and external data sources. Calculating the total fare for a multi-segment itinerary requires combining fare components from different segments, applying fare construction rules, computing taxes based on routing, and validating fare rule compliance. No single aggregate owns this logic. Domain services provide a clean home for these cross-cutting business operations.

## Travel Domain Services

### FareCalculationService

Computes the total fare for a booking by combining fare components, applying fare construction rules, and computing taxes.

- Accepts a set of segments, fare basis codes, and passenger types
- Retrieves applicable fare rules from the fare data source
- Applies fare construction logic (round-trip, circle-trip, open-jaw construction)
- Computes taxes based on routing, passenger nationality, and fare type
- Returns a FareQuote aggregate with all components, taxes, and rules
- Does not own any state -- it computes and returns results

### AvailabilitySearchService

Orchestrates availability searches across multiple GDS platforms and direct connections.

- Accepts search criteria (origin, destination, dates, passenger count, cabin class)
- Dispatches queries to multiple providers through their respective Anti-Corruption Layers
- Aggregates and normalizes responses into a unified availability model
- Applies ranking and filtering rules
- Returns sorted availability results for presentation
- Manages caching strategies for volatile availability data

### BookingValidationService

Validates that a reservation meets all requirements before ticketing.

- Checks all passenger names match required document formats
- Validates travel documents are present and not expired for all international segments
- Confirms all segments are in confirmed status
- Verifies fare quote is still valid and has not expired
- Checks payment authorization is in place
- Returns a validation result with any blocking issues

### ConnectionValidationService

Validates that connections between flight segments are feasible.

- Accepts a sequence of segments with arrival and departure times and airports
- Looks up minimum connection time (MCT) rules for each airport and terminal combination
- Considers domestic-to-international, international-to-domestic, and international-to-international connection types
- Accounts for terminal changes and inter-terminal transit times
- Returns validation result indicating which connections are valid, at-risk, or invalid

### TicketIssuanceService

Coordinates the ticket issuance process across reservation and GDS.

- Validates the reservation is ready for ticketing (delegates to BookingValidationService)
- Sends ticketing commands to the appropriate GDS through the ACL
- Records ticket numbers on the reservation
- Marks coupons for each segment
- Publishes ReservationTicketed event upon success

### RefundCalculationService

Calculates refund amounts based on fare rules and ticket usage.

- Determines which coupons have been used (flown) and which are unused
- Applies fare rule penalties (cancellation fees, change fees)
- Computes refundable taxes based on jurisdictional rules
- Returns a RefundQuote with the breakdown of refundable and non-refundable amounts

### SeatAssignmentService

Manages seat assignment across passengers and segments.

- Checks seat availability on specific flights via GDS or airline direct connect
- Applies passenger preferences (window, aisle, extra legroom)
- Enforces rules for special seats (exit row eligibility, adjacent seating for families)
- Coordinates paid seat selection with the pricing context for ancillary charges

## Domain Service Design Principles

1. **Stateless**: Domain services do not hold any state between invocations. All required data is passed in as parameters or retrieved from repositories.

2. **Named as verbs or operations**: `FareCalculationService.calculateFare()`, `ConnectionValidationService.validateConnections()` -- they represent actions, not things.

3. **Expressed in ubiquitous language**: Service names and method names use domain terminology, not technical jargon.

4. **Coordinate, do not replace**: Domain services coordinate between aggregates and call aggregate methods. They do not bypass aggregate invariants or directly manipulate aggregate internals.

5. **No infrastructure dependencies**: Domain services depend on repository interfaces and other domain abstractions, not on databases, GDS clients, or HTTP libraries directly.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Domain services coordinate operations across multiple aggregates
- [Entities](../entities/) -- Domain services operate on entities through their aggregate roots
- [Value Objects](../value-objects/) -- Domain services accept and return value objects
- [Repositories](../repositories/) -- Domain services use repositories to access aggregates
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Domain services invoke external systems through ACLs
- [Bounded Contexts](../bounded-contexts/) -- Domain services operate within a single bounded context

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7: Services. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 12: Domain Services. O'Reilly, 2021.
