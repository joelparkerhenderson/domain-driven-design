# Retail Standards

## Overview

Retail standards are industry-defined specifications that govern product identification, data exchange, transaction processing, and technology interoperability across the retail supply chain. In a Domain-Driven Design context, these standards inform value object design, serve as published languages for inter-context and external communication, and shape the anti-corruption layers that translate between external standards and internal domain models. Adherence to retail standards enables interoperability with suppliers, distributors, marketplaces, and regulatory bodies.

## GS1 Standards

GS1 is the global standards organization responsible for the most widely used supply chain standards in retail. GS1 standards are foundational to the Product Catalog Context, Inventory and Merchandising Context, and any context that identifies products or locations.

### GTIN (Global Trade Item Number)

The GTIN is the universal product identifier used worldwide. It appears on product packaging as a barcode and is the key that links products across retailers, suppliers, and marketplaces. GTIN formats include:

- **GTIN-8**: 8-digit identifier for small packages where a longer barcode will not fit.
- **GTIN-12 (UPC-A)**: 12-digit identifier predominantly used in North America. The Universal Product Code (UPC) is the most common barcode seen at retail checkout.
- **GTIN-13 (EAN)**: 13-digit identifier used internationally. The European Article Number (EAN) is the global equivalent of UPC.
- **GTIN-14**: 14-digit identifier used for cases, cartons, and pallets in the supply chain.

In DDD, the GTIN is modeled as a value object with format validation and check digit verification. The GTIN value object encapsulates the rules for valid GTIN construction and supports equality comparison and format conversion.

### GLN (Global Location Number)

A 13-digit identifier for physical and legal locations (stores, warehouses, distribution centers, corporate offices). Used in EDI messages and supply chain transactions to unambiguously identify parties and locations.

### SSCC (Serial Shipping Container Code)

An 18-digit identifier for logistics units (pallets, cases) used in the supply chain for tracking shipments. Appears on shipping labels as GS1-128 barcodes.

### GS1 Data Synchronisation Network (GDSN)

A network of certified data pools that enables trading partners to exchange standardized product data. Suppliers publish product attributes (descriptions, dimensions, weights, images, safety information) to data pools, and retailers subscribe to receive this data for their catalogs.

## PCI DSS (Payment Card Industry Data Security Standard)

PCI DSS is the security standard for organizations that handle payment card data. While PCI DSS is a security and compliance requirement, it directly impacts the domain model design of the Point of Sale Context:

- The domain model must never store raw card numbers, CVVs, or PINs.
- Payment data flows through point-to-point encryption (P2PE) devices.
- Tokenization replaces card data with non-sensitive tokens in transaction records.
- The TenderAmount value object in the POS context references tokens, not card numbers.

PCI DSS version 4.0 (effective 2024) introduces enhanced requirements for authentication, encryption, and continuous monitoring.

## ARTS (Association for Retail Technology Standards)

ARTS is a division of the National Retail Federation (NRF) that develops and maintains technology standards specific to the retail industry.

### ARTS Unified POS Data Model

A standardized data model for point-of-sale systems that defines entities, relationships, and attributes for retail transactions. The ARTS data model influences the design of the Transaction aggregate, line item entities, and tender value objects in the Point of Sale Context.

### ARTS XML and RESTful API Standards

Standardized message formats for retail data exchange, including transaction data, product data, and inventory data. These standards serve as published languages for integration between retail systems.

### ARTS Cloud Data Model

An evolution of the ARTS data model for cloud-native retail systems, supporting microservices architectures and event-driven communication patterns.

## EDI (Electronic Data Interchange)

EDI is the standardized format for electronic business document exchange between trading partners. Retail EDI uses the ANSI X12 standard in North America and UN/EDIFACT internationally.

### Key EDI Document Types

- **EDI 850 (Purchase Order)**: The retailer sends a purchase order to a supplier specifying products, quantities, delivery dates, and prices. Maps to the Purchase Order aggregate in the Inventory and Merchandising Context.
- **EDI 810 (Invoice)**: The supplier sends an invoice to the retailer for shipped merchandise. Triggers accounts payable processes.
- **EDI 856 (Advance Ship Notice / ASN)**: The supplier notifies the retailer of a pending shipment, including contents, quantities, and tracking information. Enables the retailer to prepare for receiving.
- **EDI 846 (Inventory Inquiry/Advice)**: Exchange of inventory information between trading partners.
- **EDI 860 (Purchase Order Change)**: Modifications to previously submitted purchase orders.

EDI integration is handled through an anti-corruption layer that translates between EDI segment/element structures and the retail domain's aggregates and value objects.

## NRF Standards and Initiatives

The National Retail Federation (NRF) promotes standards adoption and best practices across the retail industry. Key initiatives include:

- **NRF ARTS Standards**: The data model and API standards described above.
- **NRF Loss Prevention Standards**: Standards for shrinkage measurement and reporting.
- **NRF Technology Conferences**: Industry forums where retail technology standards are discussed and evolved (NRF Big Show, NRFtech).

## Relationships to Other Topics

- [Value Objects](../value-objects/index.md): GTIN, GLN, and SSCC are modeled as value objects.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md): Standards are translated through ACLs.
- [Integration Patterns](../integration-patterns/index.md): Standards serve as published languages.
- [Product Catalog Context](../product-catalog-context/index.md): GS1 standards for product identification.
- [Point of Sale Context](../point-of-sale-context/index.md): ARTS data model and PCI DSS compliance.
- [Inventory Merchandising Context](../inventory-merchandising-context/index.md): EDI standards for supplier integration.
- [Regulatory Compliance](../regulatory-compliance/index.md): PCI DSS bridges standards and compliance.
- [Ubiquitous Language](../ubiquitous-language/index.md): Standards inform the ubiquitous language.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- GS1. *GS1 General Specifications*. GS1 AISBL, ongoing. https://www.gs1.org/standards
- PCI Security Standards Council. *PCI Data Security Standard (PCI DSS) v4.0*. 2022. https://www.pcisecuritystandards.org
- ARTS (Association for Retail Technology Standards). *ARTS Data Model and Standards*. NRF, ongoing. https://www.nrf.com/resources/retail-technology-standards
- ANSI X12. *Electronic Data Interchange Standards*. Accredited Standards Committee X12, ongoing.
