# Passenger Management Context

## Overview

The Passenger Management Context manages the long-lived profiles of travelers: their identity, travel documents, contact information, loyalty program memberships, preferences, and special service requirements. Unlike the Reservation Context where a passenger appears as a booking-specific record, this context treats the passenger as a persistent entity that spans multiple trips and reservations. It provides the authoritative source for passenger data consumed by other contexts during booking, check-in, and travel.

## Key Domain Concepts

### Passenger Profile (Aggregate Root)

The central aggregate in this context. A Passenger Profile is identified by a system-generated PassengerProfileId and contains the traveler's personal information, documents, preferences, and loyalty data. The profile persists across reservations and is updated independently of any specific booking.

### Travel Document

An entity within the Passenger Profile representing a passport, visa, national ID card, or other government-issued document. Each document has a type, number, issuing country, nationality, issue date, and expiration date. Travel documents are validated against destination country requirements for international travel.

### Loyalty Membership

An entity representing enrollment in an airline frequent flyer program, hotel loyalty program, or car rental rewards program. Contains the program code, membership number, tier status (e.g., Silver, Gold, Platinum), and points/miles balance. Loyalty memberships are referenced during booking to apply tier benefits and accrue earnings.

### Special Service Request (SSR)

A value object representing a passenger-specific service need communicated to the airline using IATA SSR codes. Examples include:

- **WCHR**: Wheelchair required for ramp
- **BLND**: Blind passenger
- **UMNR**: Unaccompanied minor
- **MEDA**: Medical case requiring special attention
- **DPNA**: Disabled passenger with intellectual or developmental disability needing assistance

### Contact Information

A value object containing the passenger's email address, phone numbers, and emergency contact. Used for booking notifications, schedule change alerts, and check-in communication.

### Preferences

Value objects capturing traveler preferences that apply across bookings:

- **Seat Preference**: Window, aisle, extra legroom, bulkhead
- **Meal Preference**: IATA meal codes (VGML vegetarian, KSML kosher, AVML Asian vegetarian)
- **Cabin Preference**: Preferred cabin class for search and upgrade eligibility
- **Communication Preference**: Email, SMS, push notification preferences

## Invariants

- Every passenger profile must have a legal name (given name and surname at minimum)
- Each travel document number must be unique within the profile (no duplicate document entries)
- Travel documents must have valid expiration dates (not expired, unless archived)
- Loyalty membership numbers must conform to the format of the associated program
- At least one contact method (email or phone) is required for an active profile
- Date of birth, when present, must be a valid past date

## Data Privacy Considerations

Passenger data is subject to strict privacy regulations, particularly GDPR in the European Union. This context must:

- Support data access requests (right of access)
- Support data deletion requests (right to erasure / right to be forgotten)
- Maintain consent records for data processing
- Encrypt sensitive personal data at rest and in transit
- Log all access to passenger data for audit purposes
- Enforce data retention policies, archiving or deleting data after configured periods

## Integration Points

- **Reservation Context**: Provides passenger profiles for booking creation; receives per-booking passenger records
- **Itinerary Context**: Provides traveler preferences for itinerary presentation
- **Search & Availability Context**: Provides passenger preferences for search personalization
- **Payment & Billing Context**: Provides billing contact and address information
- **External loyalty systems**: Synchronizes tier status and points balances

## Domain Events

- **PassengerProfileCreated**: A new passenger profile has been registered
- **ProfileUpdated**: Passenger information has been modified
- **DocumentAdded**: A travel document has been added to a profile
- **DocumentExpiring**: A document will expire within a threshold before a scheduled trip
- **LoyaltyTierChanged**: A loyalty membership tier status has changed
- **ProfileArchived**: A passenger profile has been archived per data retention policy

## Relationships to Other Topics

- [Entities](../entities/) -- Passenger, Travel Document, and Loyalty Membership are key entities
- [Value Objects](../value-objects/) -- Preferences, SSR codes, and contact info are value objects
- [Aggregates](../aggregates/) -- PassengerProfile is the aggregate root
- [Reservation Context](../reservation-context/) -- Consumes passenger data for bookings
- [Regulatory Compliance](../regulatory-compliance/) -- GDPR and data privacy requirements shape this context

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- IATA. "Passenger Services Conference Resolutions Manual: SSR Codes."
- European Parliament. "General Data Protection Regulation (GDPR)." Regulation (EU) 2016/679.
