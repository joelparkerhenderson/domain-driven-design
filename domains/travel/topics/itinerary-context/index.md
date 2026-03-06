# Itinerary Context

## Overview

The Itinerary Context assembles confirmed travel segments into coherent trip plans, validates connections against minimum connection time requirements, checks schedule feasibility, and presents the traveler's journey as a unified document. This context consumes data from the Reservation and Pricing contexts but maintains its own model of what constitutes a valid, presentable itinerary. It is a downstream context that transforms booking data into a traveler-facing view of the trip.

## Key Domain Concepts

### Itinerary (Aggregate Root)

The central aggregate representing a complete trip plan. An Itinerary is identified by an ItineraryId and contains an ordered sequence of ItinerarySegments. The itinerary provides a consolidated view of the traveler's journey, including flight times, hotel check-in/check-out dates, car rental periods, and layover information.

### Itinerary Segment

An entity within the Itinerary aggregate representing a single leg of the journey. Each segment has:

- **Type**: Flight, hotel, car rental, train, transfer
- **Provider details**: Carrier name, flight number, hotel name, rental company
- **Timing**: Departure/arrival times (flights), check-in/check-out dates (hotels), pickup/dropoff times (cars)
- **Location**: Origin and destination airports, hotel addresses, rental locations
- **Status**: Confirmed, waitlisted, cancelled, completed
- **Confirmation codes**: PNR, hotel confirmation number, car rental confirmation

### Connection Point

A value object representing a transfer between two flight segments at an intermediate airport. Contains the airport code, arriving terminal, departing terminal, layover duration, and whether the connection meets minimum connection time requirements.

### Minimum Connection Time (MCT)

A value object representing the minimum time required for a passenger to transfer between flights at a specific airport. MCTs vary by:

- **Connection type**: Domestic-to-domestic, domestic-to-international, international-to-domestic, international-to-international
- **Terminal pair**: Different terminal combinations have different MCTs
- **Carrier**: Some carriers have carrier-specific MCTs at hub airports
- **Time of day**: MCTs may differ for connections during peak vs. off-peak hours

### Trip Summary

A read-only projection of the itinerary optimized for display, containing destination highlights, total trip duration, key dates, and a simplified segment list. The Trip Summary is a value object generated from the Itinerary aggregate.

## Invariants

- Segments must be chronologically ordered (departure of segment N+1 must be after arrival of segment N)
- Flight connections must meet minimum connection time requirements for the airport and terminal combination
- An itinerary must have at least one segment
- Segment times must be in valid time zones corresponding to their airports
- Hotel check-in dates must precede check-out dates
- Car rental pickup times must precede dropoff times

## Connection Validation

Connection validation is a key responsibility of the Itinerary Context. When segments are assembled:

1. Identify consecutive flight segments where the destination of one is the origin of the next
2. Calculate the layover time between arrival and departure
3. Look up the applicable MCT for the airport, terminal pair, and connection type
4. Flag connections that are below MCT as invalid or at-risk
5. Account for schedule changes that may affect previously valid connections

## Integration Points

- **Reservation Context**: Upstream supplier of confirmed segments (consumed via domain events)
- **Pricing & Fare Context**: Provides pricing information for display on the itinerary
- **Passenger Management Context**: Provides traveler preferences for itinerary presentation
- **Search & Availability Context**: MCT data sourced from GDS/airline schedule databases
- **Notification services**: Itinerary updates trigger notifications to travelers

## Domain Events

- **ItineraryCreated**: A new itinerary has been assembled from confirmed segments
- **ItineraryUpdated**: Segments have been added, removed, or modified
- **ConnectionAtRisk**: A schedule change has reduced connection time below MCT
- **ItineraryCompleted**: All segments have been traveled

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Itinerary is the aggregate root
- [Value Objects](../value-objects/) -- Connection Points, MCTs, and Trip Summaries are value objects
- [Reservation Context](../reservation-context/) -- Upstream supplier of confirmed segments
- [Domain Events](../domain-events/) -- Itinerary lifecycle communicated through events
- [Passenger Management Context](../passenger-management-context/) -- Provides traveler preferences

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- IATA. "Standard Schedules Information Manual (SSIM)."
- OAG. "Official Airline Guide: Minimum Connection Times Database."
