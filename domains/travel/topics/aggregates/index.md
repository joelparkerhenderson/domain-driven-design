# Aggregates in Travel

## Overview

An aggregate is a cluster of domain objects that are treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity (the aggregate root) through which all external access is managed. The aggregate root enforces invariants -- business rules that must always be true -- across all objects within the aggregate boundary. In travel, aggregates model the transactional boundaries of reservations, fare quotes, passenger profiles, itineraries, and payments.

## Relevance to Travel

Travel data involves complex relationships: a reservation has segments, passengers, and ancillary services; a fare quote contains fare components, taxes, and rules; a passenger profile includes documents, preferences, and loyalty memberships. Aggregates define which objects must be consistent together in a single transaction and which can be eventually consistent across transactions.

## Key Concepts

### Aggregate Root

The single entity through which all modifications to the aggregate pass. External code never directly modifies objects inside the aggregate -- it always goes through the root.

### Invariants

Business rules that must hold at all times within the aggregate boundary. For example:

- A reservation cannot be ticketed if any required passenger document is missing
- A fare quote total must equal the sum of base fare, taxes, and surcharges
- A payment capture amount cannot exceed the authorized amount

### Transactional Consistency

All changes within an aggregate are committed or rolled back as a single transaction. Changes between aggregates are eventually consistent, communicated via domain events.

## Travel Aggregates

### Reservation Aggregate

- **Root**: Reservation (identified by ReservationId / PNR)
- **Internal entities**: Segment, PassengerBookingRecord
- **Internal value objects**: BookingStatus, TicketingDeadline, ContactInfo, BookingSource
- **Invariants**:
  - ReservationId is unique and immutable once assigned
  - Every reservation must have at least one segment and one passenger
  - All passengers must have valid names matching travel document format
  - Reservation cannot transition to "Ticketed" unless all segments are confirmed
  - Ticketing deadline must be met or the reservation is subject to auto-cancellation
  - Cancellation is not permitted after departure of the first segment
- **Lifecycle states**: Created, Confirmed, Ticketed, InProgress, Completed, Cancelled, Expired
- **Events produced**: ReservationCreated, ReservationConfirmed, ReservationTicketed, ReservationCancelled, ReservationExpired

### FareQuote Aggregate

- **Root**: FareQuote (identified by FareQuoteId)
- **Internal value objects**: FareComponent, FareBasisCode, FareRule, TaxBreakdown, Currency, ValidityPeriod
- **Invariants**:
  - Total fare equals sum of all fare components plus all taxes and surcharges
  - Each fare component must reference a valid fare basis code
  - Fare quote has a time-limited validity and expires if not used
  - Currency must be consistent across all components within a single quote
- **Lifecycle states**: Quoted, Accepted, Expired, Superseded
- **Events produced**: FareQuoted, FareAccepted, FareExpired

### PassengerProfile Aggregate

- **Root**: PassengerProfile (identified by PassengerProfileId)
- **Internal entities**: TravelDocument, LoyaltyMembership
- **Internal value objects**: ContactInfo, MealPreference, SeatPreference, SpecialServiceRequest
- **Invariants**:
  - Each passenger profile must have at least one form of identification
  - Travel document numbers must be unique within the profile
  - Loyalty membership numbers must be valid format for the associated program
  - Date of birth, when present, must be a valid past date
- **Lifecycle states**: Active, Suspended, Archived
- **Events produced**: ProfileCreated, ProfileUpdated, DocumentAdded, DocumentExpiring

### Itinerary Aggregate

- **Root**: Itinerary (identified by ItineraryId)
- **Internal entities**: ItinerarySegment
- **Internal value objects**: ConnectionPoint, DateRange, SegmentStatus
- **Invariants**:
  - Segments must be chronologically ordered
  - Connections must meet minimum connection time requirements for the airport
  - An itinerary must have at least one segment
  - No segment departure can precede the arrival of the prior segment
- **Lifecycle states**: Draft, Confirmed, InProgress, Completed, Cancelled
- **Events produced**: ItineraryCreated, ItineraryConfirmed, SegmentCompleted, ItineraryCompleted

### Payment Aggregate

- **Root**: Payment (identified by PaymentId)
- **Internal value objects**: PaymentMethod, PaymentAmount, AuthorizationCode, PaymentReference, RefundAmount
- **Invariants**:
  - Capture amount cannot exceed authorized amount
  - Refund amount cannot exceed captured amount
  - Payment method must be validated before authorization
  - A voided payment cannot be captured or refunded
- **Lifecycle states**: Initiated, Authorized, Captured, PartiallyRefunded, FullyRefunded, Voided, Failed
- **Events produced**: PaymentAuthorized, PaymentCaptured, PaymentRefunded, PaymentVoided, PaymentFailed

## Aggregate Design Rules

1. **Keep aggregates small**: A Reservation contains Segments and PassengerBookingRecords but references Payments by PaymentId, not by containing Payment objects.

2. **Reference other aggregates by identity**: A Payment references a Reservation by ReservationId. An Itinerary references Segments by SegmentId from the Reservation context.

3. **Use domain events for cross-aggregate coordination**: When a PaymentCaptured event occurs, the Reservation context updates the booking status -- but this happens asynchronously, not in the same transaction.

4. **Design for eventual consistency between aggregates**: Accept that the Itinerary context may learn about a confirmed segment milliseconds after confirmation, not at the exact same instant.

5. **One transaction per aggregate**: Never modify multiple aggregates in a single database transaction. If you need to update both a Reservation and a Payment, use domain events to coordinate.

## Relationships to Other Topics

- [Entities](../entities/) -- Aggregate roots are entities; aggregates contain entities
- [Value Objects](../value-objects/) -- Aggregates contain value objects for immutable domain concepts
- [Domain Events](../domain-events/) -- Aggregates produce events to communicate changes
- [Repositories](../repositories/) -- One repository per aggregate root
- [Domain Services](../domain-services/) -- Coordinate operations across multiple aggregates
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context defines its own aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10: Aggregates. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 10: Aggregates. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 5: Tactical Design with Aggregates. Addison-Wesley, 2016.
