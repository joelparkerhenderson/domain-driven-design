# Context Map in Fulfillment

## Overview

A context map is a visual and conceptual representation of the relationships between bounded contexts in a system. It identifies how contexts interact, which teams are upstream or downstream, and what integration patterns govern data and control flow. In fulfillment, the context map captures the complex web of dependencies between order processing, inventory, warehouse operations, shipping, delivery, and returns -- as well as external systems like e-commerce platforms, ERP systems, and carrier APIs.

## Relevance to Fulfillment

Fulfillment is inherently a pipeline domain: orders flow in, are processed through inventory allocation and warehouse operations, are shipped via carriers, tracked through delivery, and potentially return. The context map makes these flows explicit, preventing hidden coupling and ensuring teams understand their upstream and downstream dependencies.

## Context Relationship Patterns

### Upstream/Downstream

The upstream context provides data or services that the downstream context depends on. The upstream context's changes affect the downstream context.

### Customer/Supplier

The downstream context (customer) negotiates with the upstream context (supplier) to get the interfaces it needs. Both teams collaborate on the shared interface.

### Conformist

The downstream context conforms to the upstream context's model without negotiation. The downstream team adopts the upstream model as-is.

### Anti-Corruption Layer (ACL)

The downstream context creates a translation layer to protect its domain model from the upstream context's model. This is essential when integrating with external systems whose models do not align with the internal domain.

### Shared Kernel

Two contexts share a small, explicitly defined subset of the domain model. Changes to the shared kernel require agreement from both teams.

### Open Host Service / Published Language

The upstream context provides a well-defined, stable API (open host service) using a published language (such as a canonical data format) for any number of downstream consumers.

## Fulfillment Context Map

### Internal Context Relationships

**Sales Context --> Order Processing Context (Conformist)**

- The Order Processing Context conforms to the Sales Context's order model
- Sales publishes OrderPlaced events; Order Processing consumes them without negotiation
- The fulfillment team adapts to changes in the sales order format
- Rationale: Sales is organizationally dominant; fulfillment must handle whatever orders sales produces

**Order Processing Context --> Inventory Allocation Context (Customer/Supplier)**

- Order Processing requests stock availability and allocation from Inventory Allocation
- The two teams negotiate the allocation API: what data is required, what responses are returned
- Inventory Allocation provides availability checks and reservation confirmations

**Order Processing Context --> Warehouse Operations Context (Customer/Supplier)**

- Order Processing routes validated, allocated orders to the appropriate warehouse
- Warehouse Operations receives fulfillment orders and creates pick waves
- The interface is negotiated: order format, routing rules, capacity constraints

**Inventory Allocation Context --> Warehouse Operations Context (Customer/Supplier)**

- Inventory Allocation provides stock location data to Warehouse Operations for pick optimization
- Warehouse Operations reports actual pick results back (short picks, substitutions)
- Both teams collaborate on bin-level inventory accuracy

**Warehouse Operations Context --> Shipping & Carrier Context (Customer/Supplier)**

- Warehouse Operations notifies Shipping when packages are packed and staged
- Shipping generates labels, selects carriers, and confirms dispatch
- The interface includes package dimensions, weight, destination, and service level

**Shipping & Carrier Context --> Delivery & Tracking Context (Published Language)**

- Shipping publishes shipment dispatch events in a canonical format
- Delivery & Tracking subscribes and begins tracking lifecycle management
- The published language defines tracking event types, status codes, and timestamp formats

**All Contexts --> Returns & Exceptions Context (Various)**

- Returns receives return requests initiated from customer service or customer self-service
- Returns coordinates with Inventory Allocation for restocking
- Returns coordinates with Shipping for return label generation
- Multiple upstream relationships with different integration patterns

### External System Relationships

**E-Commerce Platform --> Order Processing Context (ACL)**

- External e-commerce platforms (Shopify, Magento, WooCommerce) push orders via webhooks
- An Anti-Corruption Layer translates platform-specific order formats into the internal FulfillmentOrder model
- Each platform has its own adapter behind the ACL

**ERP System <--> Order Processing Context (ACL)**

- Enterprise resource planning systems (SAP, Oracle, NetSuite) provide order data and expect fulfillment status updates
- Bidirectional ACL translates between ERP-specific formats (IDocs, REST APIs) and the internal model
- The ACL handles differences in terminology, status codes, and data structures

**Carrier APIs <--> Shipping & Carrier Context (ACL)**

- Each carrier (UPS, FedEx, DHL, USPS) has its own proprietary API with different data models, authentication, and rate structures
- The ACL translates between internal Shipment/Package models and carrier-specific request/response formats
- This is the most heavily used ACL in the system, as carrier APIs change frequently

**Carrier Tracking APIs --> Delivery & Tracking Context (ACL)**

- Carrier tracking APIs push or provide tracking events in carrier-specific formats
- The ACL normalizes tracking events into a unified internal tracking model
- Handles differences in status codes, event granularity, and update frequency

**Payment Gateway <--> Returns & Exceptions Context (ACL)**

- Refund processing requires integration with payment gateways
- The ACL translates between internal refund models and gateway-specific refund APIs

## Context Map Diagram (Textual)

```
[E-Commerce] --ACL--> [Order Processing] --Conformist-- [Sales]
                              |
                    Customer/Supplier
                              |
                              v
                     [Inventory Allocation]
                              |
                    Customer/Supplier
                              |
                              v
                   [Warehouse Operations]
                              |
                    Customer/Supplier
                              |
                              v
[Carrier APIs] --ACL--> [Shipping & Carrier]
                              |
                     Published Language
                              |
                              v
[Carrier Tracking] --ACL--> [Delivery & Tracking]

[Returns & Exceptions] <-- Various -- [All Contexts]
[Payment Gateway] --ACL--> [Returns & Exceptions]
```

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- The nodes in the context map
- [Strategic Design](../strategic-design/) -- Context maps are a key strategic design tool
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs protect boundaries identified in the map
- [Ubiquitous Language](../ubiquitous-language/) -- Each context in the map has its own language
- [Integration Patterns](../integration-patterns/) -- Implementation of the relationships in the map
- [Event-Driven Architecture](../event-driven-architecture/) -- Events enable many of the mapped relationships

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 8: Context Mapping Patterns. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
