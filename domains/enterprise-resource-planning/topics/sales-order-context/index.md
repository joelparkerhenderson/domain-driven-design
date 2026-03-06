# Sales/Order Context in Enterprise Resource Planning

## Overview

The Sales/Order Context manages the customer-facing operations: customer management, sales order processing, pricing, discounts, order fulfillment, shipping, and returns. It represents the demand side of the ERP system.

## Core Responsibilities

- **Customer Management**: Maintaining customer records, credit limits, payment terms, and shipping preferences
- **Order Processing**: Creating, validating, and managing sales orders through their lifecycle
- **Pricing**: Applying base prices, volume discounts, customer-specific pricing, and promotional offers
- **Fulfillment**: Coordinating picking, packing, and shipping of ordered goods
- **Returns**: Processing customer returns through RMA workflow
- **Quotation Management**: Creating and tracking sales quotes with validity periods

## Aggregates

### SalesOrder Aggregate

- **Root**: SalesOrder (identified by OrderID)
- **Internal entities**: LineItem
- **Value objects**: Money, Quantity, ShippingAddress, DiscountRate, TaxRate, DeliveryTerms
- **States**: Draft → Confirmed → Picking → Packed → Shipped → Delivered → Closed
- **Invariants**: Order total within customer credit limit; line items reference valid products; cancelled lines cannot be modified; shipped orders cannot be cancelled

### Customer Aggregate

- **Root**: Customer (identified by CustomerNumber)
- **Value objects**: Address (billing, shipping), ContactInfo, PaymentTerms, CreditLimit, TaxExemptionStatus
- **Invariants**: Credit limit must be non-negative; blocked customers cannot place orders

### PriceList Aggregate

- **Root**: PriceList (identified by PriceListID)
- **Internal entities**: PriceListEntry
- **Value objects**: Money, DateRange, DiscountTier
- **Invariants**: Price entries cannot overlap for same product/date; prices must be positive

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| OrderPlaced | Customer submits order | Inventory (reservation), Finance (credit check) |
| OrderConfirmed | Order validated | Warehouse (pick list), Customer notification |
| OrderShipped | Goods dispatched | Finance (revenue recognition), Customer notification |
| OrderDelivered | Delivery confirmed | Invoice generation |
| OrderCancelled | Order cancelled | Inventory (release), Finance |
| ReturnAuthorized | RMA approved | Inventory (expect return), Finance (credit memo) |

## Integration with Other Contexts

- **Sales → Inventory**: OrderPlaced reserves stock; fulfillment triggers issuance
- **Sales → Finance**: Shipping triggers revenue recognition; invoicing creates AR
- **Customer → Procurement** (indirect): High-demand products trigger procurement through inventory
- **Pricing**: May consume external market data through ACL

## Key Workflows

### Order-to-Cash

1. Customer places order → Credit check
2. Order confirmed → Inventory reserved
3. Pick list generated → Goods picked and packed
4. Order shipped → Delivery note created
5. Invoice issued → AR created
6. Payment received → AR cleared

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six ERP bounded contexts
- [Context Map](../context-map/) — Upstream to Finance and Inventory
- [Aggregates](../aggregates/) — SalesOrder, Customer, PriceList are aggregate roots
- [Domain Services](../domain-services/) — PricingService, CreditCheckService
- [Domain Events](../domain-events/) — Sales events drive Inventory and Finance

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Monk, Ellen & Wagner, Bret. "Concepts in Enterprise Resource Planning." Cengage Learning, 2012.
