# Returns & Exceptions Context in Fulfillment

## Overview

The Returns & Exceptions Context is the bounded context responsible for managing the reverse logistics flow: processing return merchandise authorization (RMA) requests, receiving returned items at the warehouse, inspecting returned goods, determining disposition (restock, refurbish, dispose), and coordinating refunds or exchanges. It also handles fulfillment exceptions such as damaged shipments, lost packages, and mispicks that require corrective action.

## Relevance to Fulfillment

Returns are an inevitable part of fulfillment and can represent a significant operational cost. E-commerce return rates commonly range from 15% to 30%, making return processing a substantial workload. The Returns & Exceptions Context captures the domain knowledge of return authorization policies, inspection procedures, disposition rules, and exception resolution workflows, ensuring that returns are handled consistently, efficiently, and in compliance with customer-facing return policies.

## Bounded Context Boundaries

**Owns**: ReturnRequest aggregate, return authorization policies, inspection workflows, disposition rules, exception case management

**Does not own**: Refund payment processing (upstream Financial/Billing context), outbound shipping of exchange orders (Order Processing Context), inventory restocking quantities (Inventory Allocation Context)

**Upstream contexts**: Delivery & Tracking Context (delivery failures trigger exceptions), Order Processing Context (original order data for validation), customers (return initiation)

**Downstream contexts**: Inventory Allocation Context (restocked items update availability), Order Processing Context (exchange orders), Financial/Billing context (refund authorization)

## Core Capabilities

### RMA Processing

Return Merchandise Authorization (RMA) is the process of evaluating and approving a customer's return request before they ship items back.

- **Return eligibility check**: Validate that the return request falls within the return window, that the items are returnable, and that quantities do not exceed what was shipped
- **Return reason capture**: Record the customer's stated reason for return (defective, wrong item, not as described, changed mind, sizing issue)
- **Authorization decision**: Apply return policy rules to approve, deny, or partially approve the return
- **RMA number assignment**: Generate a unique RMA identifier for tracking the return through the process
- **Return label generation**: Create a prepaid or customer-paid return shipping label, depending on the return reason and policy
- **Return instructions**: Provide the customer with packaging instructions, drop-off locations, or carrier pickup scheduling

### Return Receiving

Return receiving handles the physical intake of returned items at the warehouse.

- **RMA verification**: Match incoming returns to authorized RMA records by scanning the RMA number or return label barcode
- **Item counting**: Verify that the returned items and quantities match the RMA authorization
- **Initial condition assessment**: Record the apparent condition of items upon arrival (sealed, opened, damaged, incomplete)
- **Quarantine staging**: Place received returns in a dedicated inspection area, separate from forward-flow inventory
- **Unexpected returns**: Handle returns that arrive without a valid RMA, routing them through a manual review process

### Inspection

Inspection is the process of evaluating returned items to determine their condition and appropriate disposition.

- **Condition grading**: Assess each item against a defined condition scale (new/unopened, like-new, used/acceptable, damaged/defective, unsalvageable)
- **Defect verification**: For items returned as defective, verify the reported defect and document findings
- **Functional testing**: For electronic or mechanical items, perform functional tests to confirm or deny defect claims
- **Photo documentation**: Capture photos of item condition for dispute resolution and quality analysis
- **Inspection completion**: Record inspection results and advance the return to disposition

### Disposition

Disposition determines what happens to each inspected return item.

- **Restock**: Items in new or like-new condition are returned to sellable inventory
- **Refurbish**: Items with minor cosmetic issues are sent for cleaning, repackaging, or repair
- **Liquidate**: Items that cannot be sold as new but retain value are routed to secondary sales channels
- **Dispose**: Items that are damaged beyond recovery or that are unsanitary are destroyed
- **Return to vendor**: Defective items covered by vendor warranty are returned to the supplier
- **Donation**: Items suitable for charitable donation are routed to donation programs

### Refund and Exchange Authorization

After disposition, the context determines the appropriate customer resolution.

- **Full refund**: Customer receives a complete refund for the returned items
- **Partial refund**: A restocking fee or condition-based deduction is applied
- **Store credit**: Customer receives credit rather than a payment refund
- **Exchange**: A replacement order is created for the same or a different item
- **No refund**: Return is denied based on inspection findings (e.g., customer damage, out-of-policy return)

### Exception Management

Beyond customer-initiated returns, this context handles fulfillment exceptions that require corrective action.

- **Mispick resolution**: Items incorrectly picked and shipped; coordinate reshipping the correct items
- **Damaged in transit claims**: File carrier claims for packages damaged during shipping
- **Lost package investigation**: Coordinate with carriers to locate missing shipments; reship if necessary
- **Short shipment correction**: Address cases where fewer items were delivered than expected

## Key Aggregates and Entities

- **ReturnRequest** (aggregate root): Represents a customer's return request through its full lifecycle from request to resolution
- **ReturnLineItem** (entity within ReturnRequest): An individual item being returned with reason, quantity, and disposition
- **Inspection** (entity within ReturnRequest): Inspection findings for each returned item
- **ReturnReason** (value object): Categorized reason for the return
- **Disposition** (value object): The determined outcome for an inspected item
- **ReturnLabel** (value object): Shipping label for the return shipment
- **RefundAmount** (value object): Calculated refund with any deductions applied

## Domain Events Produced

- **ReturnRequested**: A customer has initiated a return request
- **ReturnAuthorized**: A return request has been approved with RMA number and return label
- **ReturnDenied**: A return request has been rejected based on policy
- **ReturnReceived**: Returned items have arrived at the warehouse
- **ReturnInspected**: Inspection is complete with condition grades and disposition assignments
- **ReturnResolved**: The return has been fully processed with refund, exchange, or credit issued
- **ExceptionRaised**: A fulfillment exception has been identified requiring corrective action
- **ExceptionResolved**: A fulfillment exception has been resolved

## Domain Events Consumed

- **DeliveryConfirmed** (from Delivery & Tracking Context): Establishes the return window start date
- **DeliveryFailed** (from Delivery & Tracking Context): Triggers exception handling for undeliverable shipments
- **OrderReceived** (from Order Processing Context): Provides original order data for return validation

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Returns & Exceptions is one of the six core bounded contexts
- [Delivery & Tracking Context](../delivery-tracking-context/) -- Upstream context providing delivery failure events
- [Inventory Allocation Context](../inventory-allocation-context/) -- Restocked items update inventory availability
- [Domain Events](../domain-events/) -- Produces ReturnRequested, ReturnAuthorized, ReturnResolved events
- [Aggregates](../aggregates/) -- Owns the ReturnRequest aggregate
- [Domain Services](../domain-services/) -- ReturnAuthorizationService evaluates return policies

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7: Modeling Bounded Contexts. O'Reilly, 2021.
- Rogers, Dale S. & Tibben-Lembke, Ronald. "Going Backwards: Reverse Logistics Trends and Practices." Reverse Logistics Executive Council, 1999.
