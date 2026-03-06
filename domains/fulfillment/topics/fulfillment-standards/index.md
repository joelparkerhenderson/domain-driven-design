# Fulfillment Standards

## Overview

Fulfillment standards are industry-defined specifications, identifiers, label formats, and communication protocols that enable interoperability across the supply chain. In Domain-Driven Design, standards inform value object design (barcodes, labels, identifiers follow standard formats) and serve as published languages for integration between bounded contexts and external systems. Fulfillment operations depend on these standards for warehouse scanning, carrier communication, regulatory compliance, and trading partner interoperability.

## Relevance to Fulfillment

Every physical item in a fulfillment center is identified by a barcode. Every shipment carries a standardized label. Every B2B transaction follows an EDI document format. Every carrier interaction uses defined data structures for tracking numbers, routing codes, and service levels. Ignoring these standards results in scanning failures, carrier rejections, trading partner disputes, and regulatory non-compliance. The domain model must encode these standards as first-class concepts.

## GS1 Standards

GS1 is the global organization that develops and maintains supply chain standards for identification, data capture, and data sharing.

### GTIN (Global Trade Item Number)

- **Purpose**: Uniquely identifies a trade item (product) globally
- **Formats**: GTIN-8, GTIN-12 (UPC-A), GTIN-13 (EAN), GTIN-14
- **Fulfillment use**: Product identification during receiving, put-away, picking, and packing
- **Domain modeling**: Encoded as a value object with format validation and check digit verification

### SSCC (Serial Shipping Container Code)

- **Purpose**: Uniquely identifies a logistics unit (pallet, case, or package) for transport
- **Format**: 18-digit numeric code with GS1 Company Prefix and serial reference
- **Fulfillment use**: Assigned to each shipping container for tracking through the supply chain from warehouse to delivery
- **Domain modeling**: Value object on the Package or Shipment entity, generated during the packing or labeling process

### GS1-128 Barcode

- **Purpose**: Encodes supply chain data using Application Identifiers (AIs) in a standardized barcode format
- **Common AIs for fulfillment**:
  - AI 00: SSCC (shipping container identifier)
  - AI 01: GTIN (product identifier)
  - AI 02: GTIN of contained trade items
  - AI 10: Batch/lot number
  - AI 17: Expiration date
  - AI 37: Count of trade items in a container
- **Fulfillment use**: Warehouse scanning for receiving, picking, packing, and shipping; carrier scan verification
- **Domain modeling**: Barcode value object with format, encoded data, and AI parsing logic

### GS1 DataMatrix

- **Purpose**: 2D barcode that can encode more data in less space than linear barcodes
- **Fulfillment use**: Used on small items where a linear barcode does not fit; increasingly adopted for pharmaceutical and serialized item tracking
- **Domain modeling**: Barcode value object with 2D format specification

## EDI Standards

Electronic Data Interchange (EDI) defines standardized document formats for B2B communication between trading partners.

### ANSI X12 Document Types for Fulfillment

- **EDI 850 (Purchase Order)**: Inbound order from a B2B trading partner specifying items, quantities, prices, and shipping instructions. Translated into a FulfillmentOrder via the anti-corruption layer.
- **EDI 856 (Advance Ship Notice / ASN)**: Outbound notification sent to the trading partner before a shipment arrives, detailing contents, packaging hierarchy, carrier, and tracking information. Generated from the Shipment aggregate after dispatch.
- **EDI 810 (Invoice)**: Financial document sent to the trading partner accompanying or following a shipment. Generated from order and shipment data.
- **EDI 214 (Shipment Status Message)**: Carrier-provided tracking updates transmitted via EDI. Consumed by the Delivery & Tracking Context.
- **EDI 945 (Warehouse Shipping Advice)**: Confirmation from the WMS that a shipment has been dispatched.
- **EDI 944 (Warehouse Stock Receipt Advice)**: Confirmation from the WMS that inbound stock has been received.
- **EDI 940 (Warehouse Shipping Order)**: Instruction to the WMS to fulfill and ship an order.
- **EDI 943 (Warehouse Stock Transfer Shipment Advice)**: Notification of stock transfer between warehouses.

### EDIFACT

- **Purpose**: International EDI standard (UN/EDIFACT) used primarily in Europe and global trade
- **Key message types**: ORDERS (purchase order), DESADV (dispatch advice), INVOIC (invoice), IFTSTA (shipment status)
- **Fulfillment use**: International trading partner communication, cross-border shipment documentation

### EDI Transport Protocols

- **AS2 (Applicability Statement 2)**: Internet-based, encrypted, point-to-point EDI transmission
- **SFTP**: Secure file transfer for batch EDI document exchange
- **VAN (Value Added Network)**: Third-party EDI network that routes documents between trading partners

## Shipping Label Standards

### GS1 Shipping Label

- **Purpose**: Standardized label format for logistics units, compliant with GS1 specifications
- **Content**: SSCC barcode, ship-to address, ship-from address, carrier routing information, purchase order reference
- **Fulfillment use**: Applied to cartons and pallets for carrier processing and trading partner receiving

### Carrier-Specific Label Formats

- **MaxiCode**: 2D barcode used by UPS for package sorting (encodes zip code, service level, and tracking number)
- **Intelligent Mail Barcode (IMb)**: Used by USPS for mail and package tracking
- **PDF417**: 2D barcode used by FedEx and other carriers for routing and tracking data
- **ZPL (Zebra Programming Language)**: Thermal printer language used to generate carrier labels at pack stations

## Shipping Protocols and Standards

### Dimensional Weight (DIM Weight)

- **Purpose**: Pricing method that uses package volume rather than actual weight when the package is large but lightweight
- **Calculation**: (Length x Width x Height) / DIM factor (carrier-specific, typically 139 for domestic, 166 for international in inches)
- **Domain modeling**: Dimensions value object calculates dimensional weight; CarrierRate value object compares actual vs. dimensional weight for billing

### Harmonized System (HS) Codes

- **Purpose**: Internationally standardized numerical codes for classifying traded products
- **Format**: 6-digit international code, extended to 8-10 digits by individual countries
- **Fulfillment use**: Required on customs declarations for international shipments; determines duty rates and import/export restrictions
- **Domain modeling**: Value object on the product or order line, validated against HS code registries

### Incoterms

- **Purpose**: International commercial terms published by the International Chamber of Commerce defining responsibilities for shipping, insurance, and customs
- **Common terms**: FOB (Free on Board), CIF (Cost, Insurance, Freight), DDP (Delivered Duty Paid), DAP (Delivered at Place)
- **Fulfillment use**: Determines at what point responsibility for the shipment transfers between seller and buyer

## Standards in the Domain Model

These standards are encoded in the domain model as value objects with built-in validation.

- **GTIN**: Value object that validates format (8/12/13/14 digits) and check digit
- **SSCC**: Value object that validates 18-digit format and includes the GS1 Company Prefix
- **Barcode**: Value object with format (GS1-128, DataMatrix, MaxiCode) and encoded data
- **HSCode**: Value object that validates against the HS classification hierarchy
- **EDIDocument**: Value object representing a parsed EDI transaction set with segment and element validation

## Relationships to Other Topics

- [Value Objects](../value-objects/) -- Standards define the validation rules for Barcode, TrackingNumber, and other value objects
- [Integration Patterns](../integration-patterns/) -- EDI standards define published languages for B2B integration
- [Shipping & Carrier Context](../shipping-carrier-context/) -- Carrier label standards apply to the shipping workflow
- [Warehouse Operations Context](../warehouse-operations-context/) -- GS1 barcode standards apply to warehouse scanning
- [Regulatory Compliance](../regulatory-compliance/) -- HS codes and Incoterms intersect with customs regulations
- [Anti-Corruption Layer](../anti-corruption-layer/) -- EDI translations are implemented as ACL adapters

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Communication Patterns. O'Reilly, 2021.
- GS1. "GS1 General Specifications." GS1 AISBL, current edition. https://www.gs1.org/standards
- ANSI ASC X12. "Electronic Data Interchange Standards." Accredited Standards Committee X12.
- International Chamber of Commerce. "Incoterms 2020." ICC, 2019.
