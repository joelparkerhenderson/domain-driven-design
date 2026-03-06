# Value Objects in the Sales Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than
by a unique identity. Two value objects with the same attributes are considered equal
regardless of which instance is referenced. In the Sales domain, value objects represent the
descriptive, quantifiable, and measurable aspects of sales operations: prices, currencies,
percentages, scores, addresses, and date ranges. Value objects enforce business rules through
their construction, ensuring that invalid states such as a negative price or a discount
exceeding 100 percent cannot exist.

Value objects are fundamental to expressive domain modeling in Sales. Rather than using
primitive types like decimals and strings to represent business concepts, value objects
encapsulate both data and validation logic, making the domain model self-documenting and
resistant to invalid states.

## Key Concepts

**Immutability**: Once created, a value object cannot be modified. To change a price, a new
Price value object is created with the updated amount. This eliminates entire categories of
bugs related to shared mutable state and makes value objects inherently thread-safe.

**Equality by Attributes**: Two Money value objects with the same amount and currency are
equal. There is no identity to compare. This differs from entities where two Orders with
identical attributes are still distinct if they have different OrderIds.

**Self-Validation**: Value objects validate their attributes at construction time. A
DiscountPercentage value object rejects values below zero or above one hundred. A LeadScore
rejects values outside its defined range. This pushes validation to the earliest possible
point and prevents invalid data from propagating through the system.

**Side-Effect-Free Operations**: Value objects can expose operations that return new value
objects. A Price value object can provide an applyDiscount method that returns a new Price
with the discount applied, without modifying the original.

**Replaceability**: Because value objects have no identity, they can be freely replaced. If
a contract's pricing changes, the old Price value object is replaced with a new one rather
than being mutated in place.

## Sales Domain Value Objects

**Money**: Combines an amount and a Currency. Supports arithmetic operations (addition,
subtraction, multiplication) that produce new Money instances. Prevents operations across
different currencies without explicit conversion.

**Currency**: Represents a standard currency code (USD, EUR, GBP). Validated against
ISO 4217 currency codes.

**Price**: A Money value with additional context such as unit pricing, bulk pricing tiers,
or promotional pricing. Enforces that prices are non-negative.

**DiscountPercentage**: A percentage value between zero and one hundred, used in proposal
line items and contract terms.

**LeadScore**: A numerical score representing lead quality, typically ranging from zero to
one hundred. Encapsulates the scoring logic and threshold definitions for qualification
levels (cold, warm, hot).

**DateRange**: An immutable pair of start and end dates used for contract terms, campaign
durations, and reporting periods. Enforces that the end date is not before the start date.

**Address**: A composite value object containing street, city, state, postal code, and
country. Used for billing and shipping in the Order Management Context.

**Quota**: A target revenue amount for a specific time period, combining Money and DateRange.
Used in sales performance tracking and quota attainment calculations.

**ConversionRate**: A percentage representing the ratio of successful conversions at a
pipeline stage or across the entire sales cycle.

**SalesVelocity**: A composite metric combining deal count, average deal value, win rate,
and average sales cycle length.

**PaymentTerms**: Encapsulates payment schedule and conditions such as Net 30, Net 60, or
custom installment schedules. Used in orders and contracts.

## Relationship to Other Topics

- [Entities](../entities/) contain value objects as attributes.
- [Aggregates](../aggregates/) use value objects to enforce invariants.
- [Ubiquitous Language](../ubiquitous-language/) names the value objects using domain terminology.
- [Sales Standards](../sales-standards/) define formats that value objects implement.
- [Domain Events](../domain-events/) carry value objects as part of their event data.
- [Domain Services](../domain-services/) return value objects as computation results.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 6: Value Objects.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 6: Tactical Design.
- Fowler, Martin. "ValueObject." martinfowler.com, 2016.
