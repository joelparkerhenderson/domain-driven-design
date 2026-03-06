# Anti-Corruption Layer in Fulfillment

## Overview

An Anti-Corruption Layer (ACL) is a translation boundary that prevents an external system's model from leaking into a bounded context's internal domain model. It translates between the external system's data structures, protocols, and semantics and the internal ubiquitous language. In fulfillment, ACLs are essential because the domain interacts with numerous external systems -- carrier APIs, e-commerce platforms, ERP systems, and warehouse management systems -- each with its own proprietary model.

## Relevance to Fulfillment

Fulfillment systems are integration-heavy by nature. A typical fulfillment operation may integrate with:

- 5-10 carrier APIs (UPS, FedEx, DHL, USPS, regional carriers), each with different data models, authentication mechanisms, and rate structures
- Multiple e-commerce platforms (Shopify, Magento, WooCommerce, Amazon), each with different order formats and webhook schemas
- ERP systems (SAP, Oracle, NetSuite) with complex, proprietary data structures
- Third-party warehouse management systems (Manhattan Associates, Blue Yonder, Korber)

Without ACLs, the internal domain model becomes polluted with carrier-specific field names, platform-specific status codes, and ERP-specific identifiers. The result is a brittle system where a carrier API change requires modifications deep within the domain logic.

## ACL Architecture

### Structure

An ACL consists of three components:

1. **Adapter**: Handles the technical communication with the external system (HTTP calls, SOAP, EDI, webhooks)
2. **Translator**: Converts between the external system's data model and the internal domain model
3. **Facade**: Presents a clean, domain-aligned interface to the internal bounded context

### Principles

- The internal domain model never references external system concepts
- Translation happens at the boundary, not throughout the codebase
- Each external system has its own adapter; shared translation logic is avoided to prevent coupling between external integrations
- ACLs are owned by the downstream context that needs protection

## Carrier API Anti-Corruption Layers

### UPS ACL

UPS uses a SOAP/REST hybrid API with proprietary data structures.

**External model concepts**: `ShipmentRequest`, `PackageType`, `ServiceCode`, `RateResponse`, `TrackResponse`

**Translation examples**:

- UPS `ServiceCode` "01" (Next Day Air) maps to internal `ServiceLevel.OVERNIGHT`
- UPS `PackageType` "02" (Customer Supplied Package) maps to internal `PackageType.CUSTOM_BOX`
- UPS `RateResponse` with `TransportationCharges`, `ServiceOptionsCharges`, and `TotalCharges` maps to internal `CarrierRate` value object
- UPS tracking status "D" (Delivered) maps to internal `TrackingStatus.DELIVERED`

### FedEx ACL

FedEx uses REST APIs with a different terminology and structure than UPS.

**External model concepts**: `ShipRequest`, `RequestedPackageLineItem`, `ServiceType`, `RateReplyDetail`, `TrackReply`

**Translation examples**:

- FedEx `ServiceType` "PRIORITY_OVERNIGHT" maps to internal `ServiceLevel.OVERNIGHT`
- FedEx `PackagingType` "YOUR_PACKAGING" maps to internal `PackageType.CUSTOM_BOX`
- FedEx `RateReplyDetail` with `RatedShipmentDetails` maps to internal `CarrierRate` value object
- FedEx tracking `StatusDetail.Code` "DL" maps to internal `TrackingStatus.DELIVERED`

### DHL ACL

DHL uses REST APIs with European-centric conventions and metric units.

**Translation examples**:

- DHL weights in kilograms translate to internal weights with unit conversion
- DHL `ProductCode` "P" (Express Worldwide) maps to internal `ServiceLevel.INTERNATIONAL_EXPRESS`
- DHL `ShipmentEvent` with `StatusCode` maps to internal `TrackingEvent` with normalized status

### USPS ACL

USPS uses the Web Tools API with XML-based request/response formats.

**Translation examples**:

- USPS `ServiceType` "Priority Mail Express" maps to internal `ServiceLevel.EXPRESS`
- USPS tracking `TrackDetail` with `EventCode` maps to internal `TrackingEvent`
- USPS address validation responses translate to internal `Address` value objects

## E-Commerce Platform ACLs

### Shopify ACL

**External model**: Shopify orders contain `line_items`, `shipping_address`, `financial_status`, and `fulfillment_status` in Shopify-specific JSON format.

**Translation**:

- Shopify `order.line_items[].variant_id` maps to internal `OrderLine.sku` via a product mapping table
- Shopify `order.shipping_address` maps to internal `Address` value object, handling Shopify's country code format
- Shopify `order.financial_status` ("paid", "partially_paid") maps to internal `PaymentStatus` enum
- Fulfillment status updates translate from internal `FulfillmentOrder.status` back to Shopify's `fulfillment` API format

### Amazon Marketplace ACL

**Translation**:

- Amazon `AmazonOrderId` maps to internal `ChannelOrderId` on the `FulfillmentOrder`
- Amazon `OrderItem.ASIN` maps to internal SKU via product catalog lookup
- Fulfillment confirmations translate from internal `ShipmentDispatched` events to Amazon's `SubmitFeed` API format

## ERP System ACLs

### SAP ACL

**External model**: SAP uses IDocs (Intermediate Documents) with segment-based data structures and SAP-specific terminology (Sales Order, Delivery, Material).

**Translation**:

- SAP `ORDERS05` IDoc maps to internal `FulfillmentOrder` aggregate
- SAP `Material Number` maps to internal `SKU`
- SAP `Ship-To Party` maps to internal `Address` value object
- Internal fulfillment status updates translate to SAP `DESADV` (Dispatch Advice) IDocs

### NetSuite ACL

**Translation**:

- NetSuite `SalesOrder` with `item` lines maps to internal `FulfillmentOrder` with `OrderLine` objects
- NetSuite `Location` maps to internal `Warehouse` identifier
- Internal shipment confirmations translate to NetSuite `ItemFulfillment` records

## ACL Implementation Patterns

### Adapter Pattern

Each external system has a dedicated adapter class that handles communication specifics (authentication, serialization, error handling, retry logic). The adapter returns raw responses that the translator then converts.

### Strategy Pattern

When multiple carriers offer the same capability (rate quotes, label generation, tracking), a strategy pattern allows the system to select the appropriate carrier adapter at runtime while maintaining a uniform internal interface.

### Gateway Pattern

The ACL presents a gateway interface to the internal domain, abstracting all external system details. The domain code calls `carrierGateway.getRate(shipment)` without knowing which carrier is being queried.

### Event Translation

For event-based integrations (webhooks, message queues), the ACL subscribes to external events and publishes internal domain events. A Shopify order webhook triggers the ACL, which translates the payload and publishes an `OrderReceived` domain event.

## Relationships to Other Topics

- [Context Map](../context-map/) -- ACLs implement the protective boundaries identified in the context map
- [Bounded Contexts](../bounded-contexts/) -- ACLs protect bounded context model integrity
- [Integration Patterns](../integration-patterns/) -- ACLs are a key integration pattern
- [Shipping & Carrier Context](../shipping-carrier-context/) -- Heaviest ACL usage for carrier API integration
- [Order Processing Context](../order-processing-context/) -- ACLs for e-commerce and ERP integration
- [Value Objects](../value-objects/) -- ACLs translate external data into internal value objects

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Integration Patterns. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Gamma, Erich et al. "Design Patterns: Elements of Reusable Object-Oriented Software." Addison-Wesley, 1994.
