# Logistics Standards

## Overview

Logistics standards are industry-defined specifications, identifiers, communication protocols, and classification systems that enable interoperability across the transportation supply chain. In Domain-Driven Design, standards inform value object design (freight classes, HS codes, and tracking identifiers follow standard formats) and serve as published languages for integration between bounded contexts, carriers, customs authorities, and trading partners. Logistics operations depend on these standards for freight pricing, carrier communication, regulatory filing, and supply chain visibility.

## Relevance to Logistics

Every freight shipment is classified by a standard freight class. Every carrier communication follows an EDI document format. Every international shipment carries standardized customs codes. Every commercial driver's hours are recorded by a mandated ELD device. Ignoring these standards results in carrier rejections, customs delays, regulatory violations, and trading partner disputes. The domain model must encode these standards as first-class value objects with built-in validation.

## GS1 Standards

GS1 develops and maintains global supply chain standards for identification and data sharing.

### GTIN (Global Trade Item Number)

- **Purpose**: Uniquely identifies a trade item (product) globally
- **Formats**: GTIN-8, GTIN-12 (UPC-A), GTIN-13 (EAN), GTIN-14
- **Logistics use**: Product identification on bills of lading, customs declarations, and cargo manifests
- **Domain modeling**: Value object with format validation and check digit verification

### SSCC (Serial Shipping Container Code)

- **Purpose**: Uniquely identifies a logistics unit (pallet, container, or package) for transport
- **Format**: 18-digit numeric code with GS1 Company Prefix and serial reference
- **Logistics use**: Tracking containers and pallets through multi-modal transportation networks
- **Domain modeling**: Value object on the Shipment or Container entity

### GLN (Global Location Number)

- **Purpose**: Uniquely identifies a physical location (warehouse, dock, terminal) or legal entity
- **Logistics use**: Identifying pickup and delivery locations in EDI transactions and customs filings
- **Domain modeling**: Value object on facility and trading partner entities

## EDI Standards for Logistics

Electronic Data Interchange (EDI) defines standardized document formats for carrier-broker-shipper communication.

### ANSI X12 Transportation Document Types

- **EDI 204 (Motor Carrier Load Tender)**: Shipper or broker sends load details to carrier -- origin, destination, commodity, weight, equipment type, pickup/delivery dates, and rate. The primary document for tendering freight.
- **EDI 210 (Motor Carrier Freight Details and Invoice)**: Carrier sends freight invoice to shipper or broker after delivery, itemizing line-haul charges, accessorial charges, and fuel surcharges.
- **EDI 214 (Transportation Carrier Shipment Status Message)**: Carrier sends status updates at key milestones -- pickup, in transit, arrived at delivery, delivered. The primary tracking document.
- **EDI 990 (Response to a Load Tender)**: Carrier sends acceptance or rejection of an EDI 204 tender.
- **EDI 211 (Motor Carrier Bill of Lading)**: Electronic bill of lading transmitted between shipper and carrier.
- **EDI 856 (Advance Ship Notice)**: Notification of shipment contents and timing sent before arrival.

### EDIFACT

- **Purpose**: International EDI standard (UN/EDIFACT) used in global trade and ocean/air freight
- **Key message types**: IFTMIN (instruction to transport), IFTSTA (shipment status), IFTMAN (arrival notice), CUSCAR (customs cargo report)
- **Logistics use**: International carrier communication, ocean and air freight documentation, customs reporting

### EDI Transport Protocols

- **AS2**: Encrypted, point-to-point internet-based EDI transmission with signed receipts
- **SFTP**: Secure file transfer for batch EDI document exchange
- **VAN (Value Added Network)**: Third-party network routing EDI documents between trading partners
- **API-based EDI**: Modern REST/JSON alternatives to traditional X12 flat-file formats

## ELD Mandate and Telematics Standards

### ELD Technical Specifications (49 CFR Part 395 Subpart B)

- **Purpose**: FMCSA mandate requiring commercial vehicles to use certified Electronic Logging Devices to record driver hours of service
- **Data elements**: Driver identification, vehicle identification, date, time, location, engine hours, vehicle miles, duty status (driving, on-duty not driving, sleeper berth, off-duty)
- **Transfer protocols**: ELD data must be transferable to FMCSA inspectors via Bluetooth, USB, or email
- **Domain modeling**: HOSRecord value object with duty status changes, validated against ELD data format requirements

### Telematics Data Standards

- **J1939**: SAE standard for heavy-duty vehicle diagnostic communication, providing engine data, fault codes, and vehicle parameters
- **NMEA 0183 / NMEA 2000**: GPS data format standards for position, speed, and heading
- **MQTT / AMQP**: Messaging protocols commonly used for real-time telematics data transmission from vehicles to cloud systems

## NMFC Freight Classification

### National Motor Freight Classification

- **Purpose**: Standardized classification system for LTL (Less Than Truckload) freight, maintained by the National Motor Freight Traffic Association (NMFTA)
- **Classes**: 18 freight classes from Class 50 (lowest rate, high density) to Class 500 (highest rate, low density)
- **Classification factors**: Density (weight per cubic foot), handling difficulty, stowability, and liability/value
- **Logistics use**: LTL carriers use NMFC class to determine shipping rates. Incorrect classification results in carrier reclassification charges.
- **Domain modeling**: FreightClass value object validated against NMFC item numbers and classification rules

## Incoterms

### International Commercial Terms (Incoterms 2020)

- **Purpose**: Standardized trade terms published by the International Chamber of Commerce defining buyer/seller responsibilities for transportation, insurance, customs, and risk transfer
- **Key terms for logistics**:
  - **EXW (Ex Works)**: Seller makes goods available at their premises; buyer arranges all transport
  - **FOB (Free on Board)**: Seller delivers goods on board the vessel; risk transfers at ship's rail
  - **CIF (Cost, Insurance, Freight)**: Seller pays for transport and insurance to destination port
  - **DAP (Delivered at Place)**: Seller delivers goods to a named destination; buyer handles import clearance
  - **DDP (Delivered Duty Paid)**: Seller bears all costs and risks including import duties and taxes
- **Domain modeling**: Incoterm value object used in Customs & Clearance and Freight Brokerage contexts to determine responsibility allocation

## ACI/AES and Customs Filing Standards

### Advance Commercial Information (ACI)

- **Purpose**: Canada Border Services Agency (CBSA) requirement for advance electronic reporting of commercial goods arriving in Canada
- **Logistics use**: Carriers and freight forwarders must submit cargo data before arrival at the Canadian border

### Automated Export System (AES)

- **Purpose**: US Census Bureau / CBP system for filing Electronic Export Information (EEI) for goods exported from the United States
- **Logistics use**: Required for exports valued over $2,500 or requiring an export license

### Importer Security Filing (ISF / 10+2)

- **Purpose**: CBP requirement for advance cargo data on ocean shipments destined for the US, filed 24 hours before vessel loading
- **Logistics use**: Ocean freight importers must file 10 data elements; carriers must file 2 data elements

## TMS Standards

### Transportation Management System Integration

- **TMW / TrimbleTMS**: Fleet and brokerage management platform with proprietary integration APIs
- **Oracle Transportation Management**: Enterprise TMS with XML-based integration interfaces
- **Blue Yonder (JDA) TMS**: Supply chain planning and execution platform
- **SAP Transportation Management**: ERP-integrated TMS with IDoc and BAPI integration
- **Domain modeling**: TMS integrations use anti-corruption layer adapters to translate between external TMS models and internal domain models

## Standards in the Domain Model

These standards are encoded as value objects with built-in validation:

- **FreightClass**: Validates NMFC class range (50-500) and maps to classification characteristics
- **HSCode**: Validates harmonized system hierarchy (chapter, heading, subheading) and format
- **Incoterm**: Validates against the published set of Incoterms 2020 terms
- **SSCC**: Validates 18-digit format with check digit and GS1 Company Prefix
- **EDIDocument**: Validates segment structure and required elements for each transaction set type

## Relationships to Other Topics

- [Value Objects](../value-objects/) -- Standards define validation rules for FreightClass, HSCode, Incoterm, and other value objects
- [Integration Patterns](../integration-patterns/) -- EDI standards define published languages for carrier communication
- [Freight Brokerage Context](../freight-brokerage-context/) -- EDI 204/210/214/990 standards govern carrier transactions
- [Customs & Clearance Context](../customs-clearance-context/) -- HS codes, Incoterms, and ACI/AES standards govern customs filings
- [Fleet Management Context](../fleet-management-context/) -- ELD mandate and telematics standards govern vehicle data
- [Anti-Corruption Layer](../anti-corruption-layer/) -- EDI and TMS integrations are translated through ACL adapters
- [Regulatory Compliance](../regulatory-compliance/) -- Many standards exist to satisfy regulatory requirements

## References

- GS1. "GS1 General Specifications." GS1 AISBL, current edition. https://www.gs1.org/standards
- ANSI ASC X12. "Electronic Data Interchange Standards." Accredited Standards Committee X12.
- International Chamber of Commerce. "Incoterms 2020." ICC, 2019.
- National Motor Freight Traffic Association. "National Motor Freight Classification." NMFTA, current edition.
- Federal Motor Carrier Safety Administration. "ELD Technical Specifications." 49 CFR Part 395 Subpart B. US DOT.
- SAE International. "J1939 Standards Collection." SAE, current edition.
- World Customs Organization. "Harmonized System Nomenclature." WCO, current edition.
