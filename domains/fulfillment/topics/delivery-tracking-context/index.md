# Delivery & Tracking Context in Fulfillment

## Overview

The Delivery & Tracking Context is the bounded context responsible for monitoring shipments after they leave the warehouse, processing tracking events from carriers, providing status updates to customers and internal systems, capturing proof of delivery, and managing last-mile delivery exceptions. It transforms raw carrier tracking data into meaningful fulfillment events that close the loop on the order-to-delivery lifecycle.

## Relevance to Fulfillment

Once a shipment is dispatched, visibility into its journey is critical for customer satisfaction, exception management, and operational analytics. Carriers provide tracking data in varying formats, frequencies, and levels of detail. The Delivery & Tracking Context normalizes this data into a consistent model, enabling the fulfillment system to answer "Where is my package?" accurately and to detect delivery problems early enough to take corrective action.

## Bounded Context Boundaries

**Owns**: Tracking event model, delivery status state machine, proof of delivery records, delivery exception handling, estimated delivery calculations

**Does not own**: Shipment creation or label generation (Shipping & Carrier Context), return authorization (Returns & Exceptions Context), order status management (Order Processing Context)

**Upstream contexts**: Shipping & Carrier Context (provides dispatched shipment data and tracking numbers)

**Downstream contexts**: Order Processing Context (receives delivery confirmation), Returns & Exceptions Context (receives delivery failure events)

## Core Capabilities

### Tracking Event Processing

The context ingests tracking events from carrier systems and maps them to the internal tracking model.

- **Event ingestion**: Receive tracking updates via carrier webhook callbacks, API polling, or EDI 214 (Shipment Status) messages
- **Event normalization**: Map carrier-specific status codes and descriptions to a standardized set of tracking statuses
- **Event deduplication**: Prevent duplicate events from creating misleading tracking histories
- **Event sequencing**: Order events chronologically even when they arrive out of sequence
- **Multi-carrier normalization**: Present a consistent tracking experience regardless of which carrier is handling the shipment

### Standardized Tracking Statuses

The context defines a canonical set of tracking statuses used across all carriers.

- **LabelCreated**: Carrier has received shipment information but does not yet have physical possession
- **PickedUp**: Carrier has physically collected the package from the warehouse
- **InTransit**: Package is moving through the carrier's transportation network
- **OutForDelivery**: Package is on the delivery vehicle for final delivery attempt
- **Delivered**: Package has been successfully delivered to the recipient
- **DeliveryAttemptFailed**: A delivery attempt was unsuccessful (not home, access issue, weather)
- **DeliveryException**: An unexpected issue has occurred (damaged, lost, address correction needed)
- **ReturnedToSender**: Package is being returned to the origin warehouse
- **Held**: Package is being held at a carrier facility (customs hold, recipient request)

### Status Updates

The context publishes status changes to interested parties across the fulfillment system.

- **Internal event publishing**: Emit domain events for each significant status change
- **Customer notification triggers**: Signal the notification system to send email, SMS, or push notifications at key milestones (shipped, out for delivery, delivered)
- **Estimated delivery updates**: Recalculate and publish updated estimated delivery dates as the shipment progresses

### Proof of Delivery

Proof of delivery (POD) captures evidence that a package was successfully delivered.

- **Signature capture**: Record recipient signature data provided by the carrier
- **Photo proof**: Store delivery photos taken by the carrier driver
- **GPS coordinates**: Record the GPS location where delivery occurred
- **Recipient identification**: Capture the name of the person who received the package
- **Delivery timestamp**: Record the exact time of delivery as reported by the carrier
- **POD retrieval**: Fetch POD data from carrier APIs on demand or via post-delivery batch processing

### Last-Mile Management

Last-mile delivery is the final leg of the shipment journey, from the local distribution hub to the customer's door.

- **Delivery window communication**: Provide customers with estimated delivery windows based on carrier data
- **Delivery instructions**: Pass customer delivery preferences (leave at door, require signature, deliver to neighbor) to carriers
- **Redelivery scheduling**: Enable customers to reschedule failed delivery attempts
- **Alternate delivery locations**: Support delivery to pickup points, lockers, or alternate addresses

### Exception Handling

Delivery exceptions are deviations from the expected delivery flow that require intervention.

- **Damaged in transit**: Carrier reports package damage; trigger claims process and potential reshipping
- **Lost package**: Package has not been scanned for an extended period; initiate carrier investigation
- **Address correction**: Carrier cannot deliver to the provided address; request corrected address from customer
- **Customs hold**: International shipment is delayed in customs; provide documentation support
- **Refused delivery**: Recipient refuses the package; route to Returns & Exceptions Context

## Key Domain Objects

- **TrackingEvent** (value object): A single carrier tracking update with status, timestamp, location, and description
- **TrackingHistory** (entity): The ordered sequence of tracking events for a shipment
- **ProofOfDelivery** (value object): Evidence of successful delivery (signature, photo, GPS, timestamp)
- **DeliveryException** (entity): A record of a delivery problem requiring action
- **EstimatedDelivery** (value object): Calculated delivery date range based on carrier data and transit progress

## Domain Events Produced

- **TrackingEventReceived**: A new tracking update has been processed
- **DeliveryAttempted**: A carrier attempted delivery at the destination
- **DeliveryConfirmed**: A package has been successfully delivered with proof of delivery
- **DeliveryFailed**: All delivery attempts exhausted or package is undeliverable
- **DeliveryExceptionRaised**: An unexpected problem has occurred during delivery
- **EstimatedDeliveryUpdated**: The estimated delivery date has changed

## Domain Events Consumed

- **ShipmentDispatched** (from Shipping & Carrier Context): Begins tracking for a new shipment
- **LabelVoided** (from Shipping & Carrier Context): Stops tracking for a cancelled shipment

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Delivery & Tracking is one of the six core bounded contexts
- [Shipping & Carrier Context](../shipping-carrier-context/) -- Upstream context providing dispatched shipment data
- [Returns & Exceptions Context](../returns-exceptions-context/) -- Downstream context handling delivery failures
- [Domain Events](../domain-events/) -- Produces DeliveryConfirmed, DeliveryFailed events
- [Integration Patterns](../integration-patterns/) -- Carrier tracking APIs and webhooks are key integrations
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Normalizes carrier-specific tracking formats

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Communication Patterns. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
