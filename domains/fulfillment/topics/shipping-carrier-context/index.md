# Shipping & Carrier Context in Fulfillment

## Overview

The Shipping & Carrier Context is the bounded context responsible for integrating with external carriers, performing rate shopping to select optimal shipping options, generating shipping labels, creating carrier manifests, and managing the dispatch of shipments from the warehouse. It translates packed shipments from the Warehouse Operations Context into carrier-ready packages with tracking numbers, labels, and dispatch documentation.

## Relevance to Fulfillment

Carrier integration is where the fulfillment system interacts most heavily with external third-party services. Each carrier (UPS, FedEx, DHL, USPS, regional carriers) has its own API, rate structures, label formats, and business rules. The Shipping & Carrier Context absorbs this external complexity behind an anti-corruption layer, presenting a unified carrier interface to the rest of the fulfillment domain. Effective rate shopping and carrier selection directly impact shipping cost, which is often the single largest variable expense in fulfillment operations.

## Bounded Context Boundaries

**Owns**: Shipment aggregate, carrier reference data, rate shopping logic, label generation, manifest creation, dispatch workflows

**Does not own**: Packing decisions (Warehouse Operations Context), inventory allocation (Inventory Allocation Context), tracking event processing (Delivery & Tracking Context)

**Upstream contexts**: Warehouse Operations Context (provides packed shipments ready for labeling)

**Downstream contexts**: Delivery & Tracking Context (receives dispatched shipments for tracking), Order Processing Context (receives shipment status updates)

## Core Capabilities

### Carrier Integration

The context maintains integrations with multiple carriers through a unified adapter layer.

- **Carrier API adapters**: Encapsulate carrier-specific API protocols, authentication, request/response formats, and error handling behind a common interface
- **Rate retrieval**: Query carrier APIs for shipping rates based on package dimensions, weight, origin, destination, and service level
- **Label generation**: Request and receive shipping labels in the required format (ZPL for thermal printers, PDF, PNG)
- **Tracking number assignment**: Receive carrier-assigned tracking numbers during label generation
- **Void and reprint**: Cancel labels and generate replacements when errors occur
- **API resilience**: Handle carrier API downtime with circuit breakers, retries, and fallback carriers

### Rate Shopping

Rate shopping compares shipping options across carriers to find the optimal balance of cost, speed, and reliability.

- **Multi-carrier comparison**: Retrieve rates from all eligible carriers for each shipment
- **Cost optimization**: Select the lowest-cost option that meets the service level commitment
- **Transit time evaluation**: Compare estimated delivery dates across carriers
- **Surcharge analysis**: Account for residential delivery, fuel surcharges, dimensional weight adjustments, and Saturday delivery fees
- **Business rule application**: Apply carrier preferences, contract rates, volume commitments, and regional performance data

### Label Generation

Label generation produces carrier-compliant shipping labels for each package in a shipment.

- **Label formatting**: Generate labels in ZPL (for thermal printers), PDF, or image formats based on warehouse printer capabilities
- **Routing codes**: Include carrier-specific routing barcodes and sort codes
- **Return label inclusion**: Generate and include prepaid return labels when required by the order's return policy
- **International documentation**: Generate customs declarations (CN22/CN23), commercial invoices, and certificate of origin for international shipments
- **Batch label generation**: Generate labels for all packages in a pick wave or carrier manifest in a single batch operation

### Manifesting

Manifesting groups all shipments for a carrier into a single end-of-day or pre-pickup document.

- **Manifest creation**: Compile all dispatched shipments for a carrier into a manifest document
- **Carrier notification**: Transmit the manifest to the carrier's API to confirm pickup readiness
- **Manifest reconciliation**: Compare manifested shipments against carrier scan data to identify discrepancies
- **Dock scheduling**: Coordinate manifest timing with carrier pickup schedules at each warehouse dock door

### Dispatch

Dispatch is the final handoff of shipments from the warehouse to the carrier.

- **Dispatch confirmation**: Record the timestamp and carrier driver details when packages are physically picked up
- **Carrier scan verification**: Match carrier pickup scans against expected shipments to identify missing or extra packages
- **Late dispatch handling**: Escalate shipments that miss carrier cutoff times for next-day processing or alternate carrier assignment

## Key Aggregates and Entities

- **Shipment** (aggregate root): Represents a set of packages assigned to a carrier for delivery to a single destination
- **Package** (entity within Shipment): A physical container with dimensions, weight, contents, and a shipping label
- **Carrier** (entity): Reference data for a shipping carrier including API configuration and rate structures
- **ShippingLabel** (value object): Label data including tracking barcode, routing code, and printable format
- **CarrierRate** (value object): A rate quote from a carrier for a specific shipment and service level
- **TrackingNumber** (value object): Carrier-assigned tracking identifier

## Domain Events Produced

- **ShipmentCreated**: A new shipment has been created from packed packages
- **CarrierSelected**: A carrier and service level have been selected for a shipment
- **ShipmentLabeled**: Shipping labels have been generated and assigned to packages
- **ShipmentDispatched**: A shipment has been handed off to the carrier
- **ManifestCreated**: An end-of-day manifest has been generated for a carrier
- **LabelVoided**: A shipping label has been cancelled

## Domain Events Consumed

- **PackCompleted** (from Warehouse Operations Context): Triggers shipment creation and carrier selection
- **OrderCancelled** (from Order Processing Context): Triggers label voiding for unshipped orders

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Shipping & Carrier is one of the six core bounded contexts
- [Warehouse Operations Context](../warehouse-operations-context/) -- Upstream context providing packed shipments
- [Delivery & Tracking Context](../delivery-tracking-context/) -- Downstream context receiving dispatched shipments
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Carrier API adapters implement the ACL pattern
- [Domain Services](../domain-services/) -- CarrierSelectionService performs rate shopping logic
- [Integration Patterns](../integration-patterns/) -- Carrier APIs are a primary external integration point
- [Fulfillment Standards](../fulfillment-standards/) -- GS1 barcodes and SSCC labels apply to shipping

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Communication Patterns. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
