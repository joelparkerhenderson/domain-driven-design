# Integration Patterns in Fulfillment

## Overview

Integration patterns define how bounded contexts communicate with each other and with external systems. In Domain-Driven Design, integration patterns address the relationships between contexts on the context map -- shared kernel, customer-supplier, conformist, anti-corruption layer, open host service, and published language. In fulfillment, integration patterns are critical because the domain requires constant communication with external carrier APIs, ERP systems, e-commerce platforms, warehouse management systems (WMS), and EDI trading partners.

## Relevance to Fulfillment

Fulfillment sits at the intersection of multiple enterprise systems and external services. Orders arrive from e-commerce platforms and ERP systems. Inventory data flows from warehouse management systems. Shipping labels and tracking data are exchanged with carrier APIs. Financial transactions are communicated to billing systems. EDI documents are exchanged with trading partners. Each of these integrations requires a deliberate pattern to protect the fulfillment domain model from external corruption while enabling the data exchange that operations depend on.

## DDD Integration Patterns Applied to Fulfillment

### Anti-Corruption Layer (ACL)

The ACL translates between external system models and the internal fulfillment model, preventing foreign concepts from leaking into the domain.

**Fulfillment applications**:

- **Carrier API adapters**: Each carrier (UPS, FedEx, DHL, USPS) has its own API structure, error codes, and data formats. An ACL translates between carrier-specific representations and the internal Shipment, CarrierRate, and TrackingEvent models.
- **E-commerce platform adapters**: Shopify, BigCommerce, WooCommerce, and Amazon each represent orders differently. An ACL converts each platform's order format into the internal FulfillmentOrder model.
- **ERP integration adapters**: SAP, Oracle, NetSuite, and other ERPs have their own order, inventory, and product data structures. An ACL translates between ERP representations and fulfillment domain objects.

### Open Host Service (OHS)

An open host service exposes a well-defined protocol that multiple consumers can integrate with, rather than building point-to-point adapters for each consumer.

**Fulfillment applications**:

- **Order intake API**: A published API that any sales channel can use to submit orders to the fulfillment system, rather than building a custom adapter for each channel
- **Tracking API**: A public-facing API that provides normalized tracking data for any shipment, regardless of carrier
- **Inventory availability API**: An API that provides real-time available-to-sell quantities, consumed by e-commerce platforms for stock display

### Published Language

A published language defines a well-documented, shared data format used for integration between contexts or systems.

**Fulfillment applications**:

- **EDI document standards**: ANSI X12 and EDIFACT document types serve as published languages for B2B fulfillment communication
- **GS1 standards**: Barcode, label, and product identification standards provide a shared language for warehouse and logistics operations
- **Event schemas**: Published event schemas (using JSON Schema, Avro, or Protobuf) define the contract between event producers and consumers

### Customer-Supplier

In a customer-supplier relationship, the downstream context (customer) depends on the upstream context (supplier), and the upstream team accommodates downstream needs.

**Fulfillment applications**:

- **Order Processing (supplier) -> Inventory Allocation (customer)**: Order Processing publishes OrderReceived events in a format that Inventory Allocation can consume efficiently
- **Warehouse Operations (supplier) -> Shipping & Carrier (customer)**: Warehouse Operations produces PackCompleted events with all data needed for shipment creation

### Shared Kernel

A shared kernel is a small, explicitly shared subset of the domain model that two contexts agree to maintain together.

**Fulfillment applications**:

- **Product catalog reference data**: SKU definitions, product dimensions, and weight data shared between Inventory Allocation and Warehouse Operations
- **Address model**: A shared Address value object used consistently across Order Processing, Shipping, and Delivery contexts
- **Common event envelope**: A shared event structure (EventId, CorrelationId, Timestamp) used by all contexts

## External System Integrations

### Carrier APIs

- **Protocol**: REST APIs (UPS, FedEx, DHL) or SOAP (legacy carrier systems)
- **Operations**: Rate retrieval, label generation, tracking, void/cancel, manifest/close
- **Resilience**: Circuit breakers, retry with backoff, fallback carrier selection
- **Authentication**: OAuth 2.0, API keys, or certificate-based authentication depending on carrier

### E-Commerce Platform Webhooks

- **Protocol**: HTTPS webhooks (push) or REST API polling (pull)
- **Events consumed**: order.created, order.updated, order.cancelled, refund.created
- **Events published**: shipment.created, tracking.updated, delivery.confirmed
- **Verification**: Webhook signature validation (HMAC) for authenticity

### ERP Integration

- **Protocol**: REST APIs, SOAP, or file-based integration (SFTP)
- **Data exchanged**: Sales orders, purchase orders, inventory adjustments, product master data
- **Synchronization**: Near-real-time via API or scheduled batch via file transfer
- **Conflict resolution**: ERP is typically the system of record for product and financial data; fulfillment is the system of record for warehouse operations and shipment data

### Warehouse Management System (WMS)

- **Protocol**: REST APIs, database-level integration, or message queues
- **Data exchanged**: Receiving confirmations, put-away instructions, pick/pack directives, inventory snapshots
- **Pattern**: If the fulfillment system includes its own WMS capabilities, this is an internal integration. If a third-party WMS is used, an ACL translates between the WMS model and the fulfillment model.

### EDI (Electronic Data Interchange)

- **Protocol**: AS2, SFTP, or VAN (Value Added Network)
- **Key document types**:
  - EDI 850 (Purchase Order): Inbound orders from B2B trading partners
  - EDI 856 (Advance Ship Notice): Outbound shipment notification to trading partners
  - EDI 810 (Invoice): Financial document accompanying shipments
  - EDI 214 (Shipment Status): Carrier-provided tracking updates
  - EDI 945 (Warehouse Shipping Advice): WMS-to-host shipment confirmation
  - EDI 944 (Warehouse Stock Receipt Advice): WMS-to-host receiving confirmation
- **Translation**: EDI documents are translated to and from domain objects via ACL adapters

## Integration Design Guidelines

1. **Prefer asynchronous communication**: Use events and message queues between bounded contexts. Reserve synchronous API calls for queries that require immediate responses (e.g., rate shopping).

2. **Own your anti-corruption layer**: Every external integration should pass through an ACL. Never let carrier data structures or e-commerce platform models leak into the domain.

3. **Version your contracts**: API schemas, event schemas, and EDI mappings will evolve. Use explicit versioning to manage backward compatibility.

4. **Design for failure**: External systems are unreliable. Implement circuit breakers, retries, fallbacks, and dead letter queues.

5. **Monitor integration health**: Track latency, error rates, and throughput for every external integration point.

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- The primary pattern for external system integration
- [Event-Driven Architecture](../event-driven-architecture/) -- The infrastructure supporting event-based integration
- [Domain Events](../domain-events/) -- The primary mechanism for inter-context communication
- [Bounded Contexts](../bounded-contexts/) -- Integration patterns define relationships between contexts
- [Context Map](../context-map/) -- Visualizes integration relationships
- [Fulfillment Standards](../fulfillment-standards/) -- Standards that define published languages for integration

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Communication Patterns. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Richardson, Chris. "Microservices Patterns." Chapter 3: Interprocess Communication. Manning, 2018.
