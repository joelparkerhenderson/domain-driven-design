# Value Objects in Travel

## Overview

A value object is an immutable domain object defined entirely by its attributes rather than by a unique identity. Two value objects with the same attributes are considered equal and interchangeable. Value objects have no lifecycle -- they are created, used, and discarded but never modified in place. When a change is needed, a new value object is created. In travel, value objects capture the rich descriptive data that characterizes fares, currencies, dates, codes, and classifications.

## Relevance to Travel

Travel systems are rich with data that is naturally immutable and identity-free. A fare of 450.00 USD is the same regardless of which variable holds it. A fare class "Y" is the same wherever it appears. A date range from March 1 to March 15 is defined by its boundaries, not by any assigned identifier. Modeling these concepts as value objects improves clarity, prevents accidental mutation, and enables safe sharing across aggregates and contexts.

## Travel Value Objects

### Currency

Represents a monetary amount paired with its ISO 4217 currency code. A Currency of (450.00, USD) is equal to any other (450.00, USD). Currency value objects enforce precision rules (e.g., two decimal places for USD, zero for JPY) and prevent mixing of different currency codes in arithmetic operations.

### Fare Class

A single-letter code designating the booking class for a flight segment (e.g., Y for full-fare economy, J for business, F for first). Fare classes determine pricing, upgrade eligibility, and earning rates for loyalty programs. As a value object, FareClass encapsulates validation logic ensuring only valid IATA booking class letters are used.

### Fare Basis Code

An alphanumeric string encoding the fare class, restrictions, season, route type, and other pricing attributes (e.g., "YOW" for full-fare one-way economy, "BLXAP7" for restricted advance-purchase economy). The fare basis code is immutable once assigned to a fare component.

### Date Range

A pair of dates representing a period -- used for travel dates, fare validity windows, booking periods, and document expiration ranges. DateRange enforces that the start date precedes or equals the end date and provides methods for overlap detection and containment checks.

### Tax

A government-imposed charge identified by tax code, jurisdiction, and amount. Taxes are immutable once computed for a fare quote. Examples include US transportation tax (US), segment fee (ZP), September 11 Security Fee (AY), and UK Air Passenger Duty (GB).

### Airport Code

An IATA three-letter airport code (e.g., JFK, LHR, NRT). As a value object, AirportCode validates against known IATA codes and provides equality semantics based on the code string.

### Airline Code

An IATA two-letter carrier code (e.g., UA for United Airlines, BA for British Airways). Validates format and provides lookup of carrier information.

### Booking Status

An enumeration-style value object representing the current state of a reservation segment: Confirmed, Waitlisted, Cancelled, Flown, NoShow. Status transitions are enforced by the aggregate, but the status itself is an immutable snapshot.

### Seat Preference

A passenger's preferred seat characteristics: window/aisle/middle, forward/rear, exit row eligibility. As a value object, SeatPreference is defined entirely by these attributes and is shared across bookings.

### Meal Preference

A coded dietary preference (e.g., VGML for vegetarian, KSML for kosher, DBML for diabetic). Immutable and interchangeable wherever the same preference applies.

### Connection Point

A value object representing a transfer at an intermediate airport, containing the airport code, terminal information, and minimum connection time. Two connections at JFK Terminal 4 with 90-minute MCT are equal.

### Fare Rule

An immutable set of conditions: advance purchase requirement, minimum stay, maximum stay, change fee, cancellation penalty, and refund eligibility. Fare rules are attached to fare components and do not change once quoted.

### Address

A structured representation of a location: street lines, city, state/province, postal code, country code. Used for billing addresses and agency locations. Two addresses with identical fields are equal.

## Value Object Design Principles

1. **Immutability**: Value objects never change after creation. To represent a price change, create a new Currency object.
2. **Equality by attributes**: Two FareClass objects with the value "Y" are equal regardless of how they were created.
3. **Self-validation**: Value objects validate their own data at construction time. A Currency with a negative amount or an unknown currency code is rejected.
4. **Side-effect-free operations**: Methods on value objects return new value objects rather than modifying state. Adding two Currency objects returns a new Currency.
5. **Replaceability**: Value objects can be freely substituted when attributes match.

## Relationships to Other Topics

- [Entities](../entities/) -- Entities contain value objects; choosing between entity and value object is a key modeling decision
- [Aggregates](../aggregates/) -- Value objects live inside aggregates and are part of the consistency boundary
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs convert raw external data into well-typed value objects
- [Pricing & Fare Context](../pricing-fare-context/) -- Heavy use of value objects for fares, taxes, and rules
- [Travel Standards](../travel-standards/) -- IATA codes and standards inform value object design

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6: Value Objects. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Value Objects. O'Reilly, 2021.
