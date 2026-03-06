# Omnichannel Context

## Overview

The Omnichannel Context is the bounded context responsible for orchestrating customer experiences across multiple retail channels including physical stores, e-commerce websites, mobile applications, marketplaces, and social commerce. This context manages capabilities such as buy-online-pick-up-in-store (BOPIS), ship-from-store, endless aisle, unified cart, and cross-channel order management. For retailers competing on convenience and seamless experience, the Omnichannel Context often contains core subdomain capabilities that represent significant competitive differentiation.

## Core Concepts

**Channel**: A pathway through which a customer interacts with the retailer. Channels include brick-and-mortar stores, the retailer's e-commerce website, native mobile applications, third-party marketplaces (Amazon, Walmart Marketplace), social commerce platforms, and call centers. Each channel has its own characteristics for product display, pricing, and fulfillment, but the Omnichannel Context ensures coordination.

**Order**: The central entity representing a customer's purchase request, regardless of the originating channel. An order contains line items, a delivery method, a delivery address or pickup location, and payment information. Unlike a POS transaction (which is synchronous and immediate), an order may have an extended lifecycle spanning placement, payment authorization, fulfillment, shipment, delivery, and potential return.

**Fulfillment Method**: The mechanism by which merchandise reaches the customer. Methods include ship-from-warehouse (traditional e-commerce), ship-from-store (using store inventory for online orders), buy-online-pick-up-in-store (BOPIS), curbside pickup, same-day delivery, and locker pickup. Each method has different inventory requirements, lead times, and cost structures.

**Buy-Online-Pick-Up-In-Store (BOPIS)**: The customer places an order online and picks it up at a selected store. This requires real-time inventory visibility, store notification workflows, picking and staging processes, customer notification when ready, and a pickup checkout process at the store. BOPIS has become a critical retail capability.

**Ship-From-Store**: Online orders are fulfilled from store inventory rather than warehouse inventory. This turns stores into mini-fulfillment centers, improving delivery speed and reducing shipping costs. Ship-from-store requires accurate store inventory data, store associate picking workflows, carrier integration, and rules for which stores can fulfill which orders.

**Endless Aisle**: When a product is out of stock in a customer's local store, the system enables the customer to order it from another location (another store or warehouse) for delivery or pickup. Endless aisle extends the visible assortment beyond what a single store can physically carry.

**Unified Cart**: A shopping cart that persists across channels. A customer can add items to their cart on a mobile app, review the cart on a desktop website, and complete the purchase in-store. The unified cart requires cross-channel session management and real-time synchronization.

**Order Orchestration**: The process of routing an order through fulfillment steps, managing state transitions, handling exceptions (out-of-stock at assigned location, carrier delays), and coordinating with the customer. Orchestration includes splitting orders across multiple fulfillment locations when no single location can fulfill the entire order.

## Aggregates

The **Order Aggregate** is rooted at the Order entity and contains order line items, fulfillment assignments, and delivery tracking. Invariants: all line items must have a fulfillment assignment, payment must be authorized before fulfillment begins, and cancellation is only permitted before shipment.

## Domain Events

- OrderPlaced: A customer has submitted an order through any channel.
- OrderFulfillmentAssigned: An order has been assigned to a fulfillment location and method.
- BOPISReadyForPickup: A BOPIS order has been picked and staged for customer collection.
- ShipFromStoreDispatched: A ship-from-store order has been handed to a carrier.
- OrderDelivered: An order has been confirmed as delivered to the customer.
- OrderCancelled: An order has been cancelled before fulfillment completion.

## Domain Services

**FulfillmentRoutingService**: Determines the optimal fulfillment location and method for an order. Considers inventory availability across locations, proximity to customer, shipping costs, delivery speed commitments, and store workload. This is often a core domain service that encodes the retailer's fulfillment strategy.

**ChannelReconciliationService**: Ensures consistency of product availability, pricing, and customer data across channels.

## Integration with Other Contexts

- **Inventory and Merchandising Context** (upstream): Provides real-time stock availability across all locations.
- **Product Catalog Context** (upstream): Provides product data for display across channels.
- **Pricing and Promotions Context** (upstream): Provides channel-appropriate pricing.
- **Customer and Loyalty Context**: Provides customer identification, preferences, and loyalty data for personalization.
- **Point of Sale Context**: Handles BOPIS pickup checkout and in-store returns of online orders.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): Omnichannel is a core retail bounded context.
- [Aggregates](../aggregates/index.md): Order is the primary aggregate.
- [Domain Events](../domain-events/index.md): OrderPlaced, BOPISReadyForPickup, and related events.
- [Domain Services](../domain-services/index.md): FulfillmentRoutingService is a key domain service.
- [Inventory Merchandising Context](../inventory-merchandising-context/index.md): Real-time inventory feeds omnichannel.
- [Event-Driven Architecture](../event-driven-architecture/index.md): Omnichannel orchestration relies heavily on events.
- [Integration Patterns](../integration-patterns/index.md): Cross-context integration is central to omnichannel.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Bell, David, Santiago Gallino, and Antonio Moreno. "How to Win in an Omnichannel World." *MIT Sloan Management Review*, 2014.
- National Retail Federation. *Omnichannel Retail Index*. NRF, ongoing.
