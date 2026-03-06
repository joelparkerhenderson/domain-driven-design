# Customs & Clearance Context in Logistics

## Overview

The Customs & Clearance Context is the bounded context responsible for managing import/export documentation, customs declarations, tariff classification, duty calculation, trade compliance screening, and interaction with customs authorities. It ensures that international freight movements comply with the trade regulations of origin, transit, and destination countries before goods cross borders.

## Relevance to Logistics

International logistics is governed by a complex web of trade regulations, tariff schedules, and security programs. Every shipment crossing a national border requires accurate customs documentation: HS code classification, declared value, country of origin, and applicable trade agreement claims. Errors in customs filings result in shipment delays, penalty assessments, seizure of goods, or loss of trusted trader status. This context absorbs the regulatory complexity of international trade, presenting a clean domain interface to other contexts.

## Bounded Context Boundaries

**Owns**: CustomsDeclaration aggregate, ClearanceRequest aggregate, TradeCompliance checks, HS code classification, duty calculation, filing with customs authorities

**Does not own**: Freight pricing (Freight Brokerage Context), route planning (Route Optimization Context), physical transportation (Fleet Management Context)

**Upstream contexts**: Freight Brokerage Context (provides international shipment details requiring customs processing)

**Downstream contexts**: Freight Brokerage Context (receives clearance status enabling shipment release), Route Optimization Context (receives clearance status for cross-border route planning)

## Core Capabilities

### Customs Declaration Management

- **Declaration creation**: Assembling customs declaration documents from shipment data including commodity descriptions, HS codes, declared values, quantities, weights, country of origin, and parties (importer, exporter, consignee)
- **HS code classification**: Assigning or validating Harmonized System codes for each commodity in the shipment, determining the applicable duty rate and import/export restrictions
- **Valuation**: Determining the customs value of goods using WTO valuation methods (transaction value, deductive value, computed value)
- **Documentation assembly**: Gathering required supporting documents including commercial invoice, packing list, certificate of origin, phytosanitary certificates, and product-specific permits

### Filing and Communication

- **Electronic filing**: Submitting customs declarations electronically to the relevant authority -- CBP ACE (US), CBSA CARM (Canada), EU CDS, or other national customs systems
- **Pre-arrival filing**: Submitting advance cargo information (ACI in Canada, ISF 10+2 in the US, ENS in the EU) before cargo arrives at the border
- **Response processing**: Receiving and interpreting customs authority responses including release, hold, inspection request, and rejection with reason codes
- **Amendment filing**: Submitting corrections to previously filed declarations when errors are discovered

### Duty and Tariff Calculation

- **Duty computation**: Calculating import duties based on HS code, customs value, country of origin, and applicable tariff schedule (HTS in the US, EU Common Customs Tariff)
- **Free trade agreement application**: Determining eligibility for preferential duty rates under trade agreements (USMCA, EU-UK TCA, RCEP, CPTPP) and applying appropriate rates
- **Anti-dumping and countervailing duties**: Identifying commodities subject to special duties and calculating additional assessments
- **Bonded entry management**: Managing goods entered into bonded warehouses or foreign trade zones where duties are deferred

### Trade Compliance

- **Denied party screening**: Checking all parties (shipper, consignee, end user) against government restricted and denied party lists (SDN, Entity List, Denied Persons List)
- **Export control classification**: Determining whether goods require an export license based on EAR (Export Administration Regulations) or ITAR classification
- **Embargo and sanctions compliance**: Screening shipments against country-level embargo and sanctions programs
- **Record keeping**: Maintaining customs records for the legally required retention period (typically 5 years in the US under 19 CFR Part 163)

## Key Aggregates and Entities

- **CustomsDeclaration** (aggregate root): A formal filing with line items, HS codes, declared values, duty calculations, and filing status
- **ClearanceRequest** (aggregate root): A request for goods to be released across a border, tracking the filing, authority response, and resolution
- **DeclarationLineItem** (entity within CustomsDeclaration): A single commodity line with HS code, quantity, value, and country of origin
- **TradePartner** (entity): An importer or exporter with compliance screening history and trusted trader program status
- **HSCode** (value object): A harmonized system classification code with chapter, heading, and subheading
- **DutyAmount** (value object): A calculated duty with rate type (ad valorem, specific, compound), rate, and computed amount
- **Incoterm** (value object): A standardized trade term defining buyer/seller responsibilities

## Domain Events Produced

- **DeclarationFiled**: A customs declaration has been submitted to the authority
- **CustomsCleared**: Goods have been approved for border crossing
- **CustomsHeld**: Goods have been flagged for inspection or additional documentation
- **DutyAssessed**: Duty amount has been calculated and confirmed
- **ComplianceViolationDetected**: A trade compliance issue has been found

## Domain Events Consumed

- **LoadTendered** (from Freight Brokerage Context): Triggers customs pre-filing for international loads
- **TenderAccepted** (from Freight Brokerage Context): Confirms international shipment is proceeding and full declaration should be prepared

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Customs & Clearance is one of the six core bounded contexts
- [Freight Brokerage Context](../freight-brokerage-context/) -- Partnership for international shipment processing
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Government customs system integrations use ACL adapters
- [Value Objects](../value-objects/) -- HSCode, Incoterm, DutyAmount are key value objects
- [Logistics Standards](../logistics-standards/) -- HS codes, Incoterms, ACI/AES standards govern this context
- [Regulatory Compliance](../regulatory-compliance/) -- CBP, C-TPAT, export controls, and sanctions shape this context

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6: Bounded Contexts. O'Reilly, 2021.
- World Customs Organization. "International Convention on the Harmonized Commodity Description and Coding System." WCO, current edition.
- International Chamber of Commerce. "Incoterms 2020." ICC, 2019.
- US Customs and Border Protection. "Customs Regulations of the United States." 19 CFR.
