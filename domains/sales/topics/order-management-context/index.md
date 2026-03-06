# Order Management Context

## Overview

The Order Management Context handles order creation from closed deals, order processing,
and the fulfillment handoff to downstream systems. This bounded context bridges the sales
process and the fulfillment process, transforming a successfully closed opportunity into a
confirmed, processable order with specific products, quantities, pricing, and delivery
requirements. The Order Management Context ensures that the commitments made during deal
negotiation are accurately captured and transmitted for fulfillment.

Order Management is classified as a generic subdomain because order creation and processing
follow well-established commercial patterns common across industries. The key domain logic
involves translating deal terms into order specifications and managing the handoff to
fulfillment systems.

## Key Concepts

**Order Creation**: The process of generating an order from a closed deal. When the
Opportunity Management Context emits a DealClosedWon event, the Order Management Context
creates an order based on the proposal terms, including line items, pricing, discounts,
payment terms, and delivery requirements.

**Order Line Items**: Individual products or services within an order, each specifying the
item, quantity, unit price, and applicable discounts. The Order aggregate enforces that the
total order value equals the sum of line item subtotals adjusted for order-level discounts
and taxes.

**Order Processing**: The workflow that an order follows from creation through validation,
approval, and confirmation. Processing may include credit checks, inventory verification,
pricing validation, and management approval for orders exceeding certain thresholds.

**Fulfillment Handoff**: The event-driven transition of a confirmed order from the Sales
domain to the Fulfillment domain. The FulfillmentHandoffCompleted event signifies that
the order has been successfully transmitted and acknowledged by the fulfillment system.
This is a critical integration point that crosses domain boundaries.

**Order Status Tracking**: Orders progress through defined statuses: Draft, Submitted,
Validated, Approved, Confirmed, Fulfillment Requested, and Fulfilled. Each status
transition is recorded as a domain event for audit and tracking purposes.

**Order Amendments**: The process of modifying an order after creation but before
fulfillment. Amendments may involve adding or removing line items, adjusting quantities,
or changing delivery details. Amendment rules depend on the current order status.

**Payment Terms**: The conditions governing when and how payment is expected, such as
Net 30, Net 60, or milestone-based payment schedules. Payment terms are captured as
value objects within the Order aggregate.

## Aggregates and Entities

- **Order** (Aggregate Root): Contains order lines, billing address, shipping address,
  payment terms, and status. Enforces total calculation invariants and valid status
  transitions.

- **OrderLine** (Entity): A line item within an order specifying product, quantity, unit
  price, and discount.

- **Money** (Value Object): Monetary amounts with currency for pricing and totals.

- **Address** (Value Object): Billing and shipping address information.

- **PaymentTerms** (Value Object): Payment schedule and conditions.

## Domain Events

- OrderCreated: A new order has been generated from a closed deal.
- OrderLineAdded: A line item has been added to an order.
- OrderSubmitted: An order has been submitted for processing.
- OrderValidated: An order has passed all validation checks.
- OrderApproved: An order has received required management approval.
- OrderConfirmed: An order has been confirmed and is ready for fulfillment.
- OrderAmended: An order has been modified after initial creation.
- FulfillmentHandoffRequested: The order has been submitted to the fulfillment system.
- FulfillmentHandoffCompleted: The fulfillment system has acknowledged receipt.

## Context Relationships

- **Upstream from Opportunity Management**: Receives DealClosedWon events to initiate
  order creation based on accepted proposal terms.

- **Downstream to Fulfillment Domain**: Transmits confirmed orders to the external
  Fulfillment domain via the fulfillment handoff.

- **Downstream to Account Management**: Order confirmations update account records
  with purchase history.

- **Downstream to Sales Analytics**: Order data feeds into revenue reporting and
  sales performance metrics.

- **Anti-Corruption Layer to ERP**: External ERP systems receive order data through
  an ACL that translates between the Sales domain's order model and the ERP's
  sales order format.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
- Hohpe, Gregor and Woolf, Bobby. "Enterprise Integration Patterns."
  Addison-Wesley, 2003.
