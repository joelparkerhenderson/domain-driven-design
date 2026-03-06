# Last-Mile Delivery Context in Logistics

## Overview

The Last-Mile Delivery Context is the bounded context responsible for managing the final transportation segment from a distribution hub, depot, or cross-dock facility to the end recipient's address. It encompasses delivery route construction, driver dispatch, stop sequencing, real-time delivery tracking, proof of delivery capture, failed delivery handling, and customer notification. Last-mile delivery is the most visible and often the most expensive segment of the logistics chain.

## Relevance to Logistics

Last-mile delivery typically accounts for 40-55% of total shipping costs due to the inefficiency of navigating local roads, making frequent stops, and handling individual deliveries. Customer expectations for speed, transparency, and flexibility continue to rise. This context must balance delivery density (stops per route) against time window commitments, manage a fleet of delivery vehicles across service areas, provide real-time visibility to customers, and capture proof of delivery to close the delivery lifecycle. The quality of last-mile execution directly determines customer satisfaction and repeat business.

## Bounded Context Boundaries

**Owns**: DeliveryRoute aggregate, DeliveryStop aggregate, ProofOfDelivery aggregate, driver dispatch, customer notifications, delivery attempt tracking

**Does not own**: Long-haul route planning (Route Optimization Context), vehicle maintenance (Fleet Management Context), freight pricing (Freight Brokerage Context), yard operations (Yard Management Context)

**Upstream contexts**: Yard Management Context (provides loaded trailer readiness), Route Optimization Context (provides delivery route plans for complex multi-hub scenarios)

**Downstream contexts**: Customer-facing tracking systems (receive delivery status updates), billing systems (receive completed delivery confirmations)

## Core Capabilities

### Delivery Route Management

- **Route construction**: Building delivery routes from a pool of pending delivery stops, optimizing for total distance, total time, or delivery density within time window constraints
- **Stop sequencing**: Ordering stops within a route to minimize drive time while ensuring each delivery is attempted within its committed time window
- **Route balancing**: Distributing stops across available drivers to equalize workload and ensure no driver is overloaded or underutilized
- **Dynamic re-sequencing**: Adjusting stop order in real-time when conditions change (traffic, failed delivery, priority add-on)

### Driver Dispatch

- **Driver assignment**: Matching delivery routes to available drivers based on vehicle type, delivery area familiarity, certification requirements (e.g., hazmat), and shift availability
- **Mobile dispatch**: Pushing route assignments and stop details to driver mobile devices with turn-by-turn navigation integration
- **Real-time monitoring**: Tracking driver position, route progress, and delivery completion rate throughout the delivery shift
- **Exception escalation**: Alerting dispatch when a driver falls behind schedule, encounters access issues, or reports vehicle problems

### Delivery Execution

- **Delivery attempt recording**: Capturing the outcome of each delivery attempt (delivered, not home, refused, access problem, wrong address) with timestamp and location
- **Proof of delivery capture**: Recording delivery confirmation through multiple evidence types: recipient signature on mobile device, photograph of delivered package, GPS coordinate at delivery location, and delivery timestamp
- **Failed delivery handling**: Managing the workflow for failed deliveries including reattempt scheduling, alternative delivery location, hold at depot, or return to sender
- **Special handling**: Managing deliveries requiring age verification, signature requirement, inside delivery, or assembly

### Customer Communication

- **Proactive notifications**: Sending automated notifications at key delivery milestones: out for delivery, approaching (30 minutes), delivered, failed attempt
- **ETA updates**: Providing real-time estimated delivery time windows that narrow as the driver approaches
- **Delivery preferences**: Capturing and applying customer preferences for delivery location (front door, side door, neighbor), time windows, and communication channel (SMS, email, app push)
- **Feedback collection**: Soliciting delivery experience feedback from recipients after successful delivery

## Key Aggregates and Entities

- **DeliveryRoute** (aggregate root): An ordered sequence of delivery stops assigned to a driver for a delivery shift, with route status and progress tracking
- **DeliveryStop** (entity within DeliveryRoute): A single delivery destination with address, time window, package details, delivery instructions, and attempt history
- **ProofOfDelivery** (aggregate root): Evidence of successful delivery including signature data, photographs, GPS coordinates, timestamps, and recipient identification
- **DeliveryAttempt** (entity within DeliveryStop): A single attempt to deliver at a stop with outcome, timestamp, evidence, and reason code for failures
- **DeliveryAddress** (value object): A structured address with delivery-specific attributes like access codes, floor, and special instructions
- **DeliveryTimeWindow** (value object): A customer-facing time range for expected delivery
- **SignatureImage** (value object): Captured signature binary data with timestamp

## Domain Events Produced

- **DeliveryRouteDispatched**: A delivery route has been assigned to a driver and released for execution
- **DeliveryAttempted**: A delivery attempt has been made (with outcome: delivered, failed, refused)
- **DeliveryCompleted**: A delivery has been successfully completed with POD captured
- **DeliveryFailed**: A delivery could not be completed after all attempts
- **CustomerNotified**: A notification has been sent to the recipient with status update or ETA
- **DeliveryRescheduled**: A failed delivery has been rescheduled for a future date

## Domain Events Consumed

- **LoadingCompleted** (from Yard Management Context): Triggers delivery route construction from loaded trailer contents
- **ETAUpdated** (from Route Optimization Context): Updates delivery time windows for customer notifications

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Last-Mile Delivery is one of the six core bounded contexts
- [Yard Management Context](../yard-management-context/) -- Receives loaded trailer readiness triggers
- [Route Optimization Context](../route-optimization-context/) -- May receive pre-computed delivery route plans
- [Domain Services](../domain-services/) -- DeliveryRouteBuilderService and DeliveryDispatchService operate here
- [Domain Events](../domain-events/) -- Delivery events drive customer notifications and completion workflows
- [Subdomains](../subdomains/) -- Last-mile optimization is a core subdomain

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6: Bounded Contexts. O'Reilly, 2021.
- Boyer, Kenneth K., Prud'homme, Andrea M. & Chung, Wenming. "The Last Mile Challenge: Evaluating the Effects of Customer Shopping and Delivery Characteristics on the Last Mile." Journal of Business Logistics, 2009.
- Boysen, Nils, Fedtke, Stefan & Schwerdfeger, Stefan. "Last-Mile Delivery Concepts: A Survey from an Operational Research Perspective." OR Spectrum, 2021.
