# ERP Standards in Enterprise Resource Planning

## Overview

ERP standards define the data formats, code systems, and communication protocols used to exchange business information between internal contexts and external partners. These standards inform value object design, published languages, and anti-corruption layers.

## Data Exchange Standards

### EDI (Electronic Data Interchange)

Structured transmission of business documents between organizations using standardized formats.

- **ANSI X12**: North American standard
  - 850 — Purchase Order
  - 810 — Invoice
  - 856 — Advance Ship Notice (ASN)
  - 820 — Payment Order/Remittance Advice
  - 997 — Functional Acknowledgment
- **UN/EDIFACT**: International standard used in Europe and Asia
- **DDD mapping**: ACL translates between EDI transaction sets and internal domain aggregates (PurchaseOrder, Invoice)

### UBL (Universal Business Language)

OASIS standard for XML-based business documents.

- Covers: Orders, invoices, dispatch advice, credit notes, remittance
- **DDD mapping**: UBL documents serve as published language for B2B integration; ACL translates to internal domain objects

### XBRL (eXtensible Business Reporting Language)

Standard for electronic communication of business and financial data.

- Used for financial reporting to regulators (SEC, HMRC)
- **DDD mapping**: Finance context translates internal financial statements to XBRL format via ACL

### ISO 20022

Standard for financial messaging between financial institutions.

- Covers: Payments, securities, trade finance, foreign exchange
- **DDD mapping**: ACL translates between ISO 20022 messages and internal Payment/BankTransaction value objects

## Code Systems

### ISO 4217 (Currency Codes)

Three-letter codes for currencies: USD, EUR, GBP, JPY.

**DDD mapping**: Money value object uses ISO 4217 currency code.

### ISO 3166 (Country Codes)

Two-letter codes for countries: US, GB, DE, JP.

**DDD mapping**: Address value object uses ISO 3166 country code.

### Incoterms (International Commercial Terms)

Standard trade terms published by the International Chamber of Commerce: FOB, CIF, EXW, DDP.

**DDD mapping**: DeliveryTerms value object.

### HS Codes (Harmonized System)

International standard for classifying traded products. Used for customs and trade compliance.

**DDD mapping**: ProductClassification value object for international trade.

### UNSPSC (United Nations Standard Products and Services Code)

Hierarchical classification of products and services for procurement.

**DDD mapping**: ProductCategory value object for procurement and sourcing.

## Industry-Specific Standards

### GS1 (Global Standards)

- **GTIN** (Global Trade Item Number): Product identification (barcodes)
- **GLN** (Global Location Number): Location identification
- **SSCC** (Serial Shipping Container Code): Logistics unit identification
- **DDD mapping**: SKU and warehouse location value objects

### SWIFT

Financial messaging network for bank-to-bank transactions. MT messages (MT940 for statements, MT103 for transfers).

**DDD mapping**: ACL translates SWIFT messages to internal BankTransaction entities.

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) — ACLs translate between standards and internal models
- [Integration Patterns](../integration-patterns/) — Standards define the published languages
- [Value Objects](../value-objects/) — Standard codes modeled as value objects
- [Finance/Accounting Context](../finance-accounting-context/) — XBRL, ISO 20022 for financial integration
- [Procurement Context](../procurement-context/) — EDI, UBL for supplier integration

## References

- ASC X12. "EDI Transaction Standards." https://x12.org/
- UNECE. "UN/EDIFACT." https://unece.org/trade/uncefact/introducing-unedifact
- OASIS. "Universal Business Language (UBL)." https://www.oasis-open.org/committees/ubl/
- XBRL International. "XBRL Specification." https://www.xbrl.org/
- ISO. "ISO 20022 Financial Messaging." https://www.iso20022.org/
- ICC. "Incoterms 2020." International Chamber of Commerce.
- GS1. "GS1 General Specifications." https://www.gs1.org/
