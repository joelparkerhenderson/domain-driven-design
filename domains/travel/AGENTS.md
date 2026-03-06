# Travel - Domain Driven Design (DDD)

The "Travel" domain is characterized by high complexity due to volatile availability, multi-party integrations (airlines, hotels, GDS), and long-running business processes.

## 1. Strategic Design: Subdomains in Travel

Strategic design helps prioritize where to spend engineering effort by categorizing different business areas

## 2. Bounded Contexts & Ubiquitous Language

A single "Trip" or "Booking" or "User" means different things depending on where it is used. Bounded Contexts define these linguistic and logical boundaries:

## Tactical Patterns for Travel Systems

These patterns manage the internal complexity of a Bounded Context:

- Aggregates: The Reservation acts as an Aggregate Root. It ensures that if you add a "Flight Segment," the "Total Price" and "Tax" are updated atomically to keep the state consistent.
- Entities: Objects with identity, such as a Passenger or a specific FlightInstance.
- Value Objects: Objects defined by their attributes rather than identity, like Currency, FareClass, or a DateRange.
- Domain Events: Critical for integration. When a user completes a purchase, the system fires an OrderConfirmed event, which triggers the Notification Context to send an email and the Billing Context to issue an invoice.

## Integration Challenges

Travel systems often rely on external Global Distribution Systems (GDS) like Amadeus or Sabre. In DDD, you use an Anti-Corruption Layer (ACL) to translate the messy, legacy data formats from these external providers into your clean, internal Ubiquitous Language.
