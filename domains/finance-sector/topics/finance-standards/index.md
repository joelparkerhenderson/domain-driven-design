# Finance Standards

## Overview

Finance standards define the data formats, message structures, communication protocols, and security requirements used to exchange financial information between systems. In a DDD finance system, these standards inform the design of value objects, published languages, anti-corruption layers, and integration interfaces.

## Relevance to Finance

Financial systems do not operate in isolation. They exchange data with payment networks, trading venues, regulators, credit bureaus, custodians, and counterparties worldwide. Standards enable interoperability, reduce integration costs, and satisfy regulatory requirements. Understanding these standards is essential for designing bounded context interfaces and anti-corruption layers.

## Messaging and Data Standards

### ISO 20022

The universal financial industry message scheme, providing a common platform for financial messaging across payments, securities, trade finance, and foreign exchange.

- **Structure**: XML-based messages organized into business domains (pain = payments initiation, pacs = payments clearing and settlement, camt = cash management, sese = securities settlement)
- **Key messages**:
  - `pain.001` -- Customer credit transfer initiation (payment order)
  - `pacs.008` -- Financial institution credit transfer (interbank payment)
  - `camt.053` -- Bank-to-customer account report (statement)
  - `sese.023` -- Securities settlement transaction instruction
- **DDD mapping**: ISO 20022 message types serve as published languages for inter-context and external communication. Internal domain models translate to/from ISO 20022 through anti-corruption layers.
- **Adoption**: Mandated by SWIFT for cross-border payments migration (completed by November 2025); adopted by payment market infrastructures globally

### SWIFT

The Society for Worldwide Interbank Financial Telecommunication operates the global messaging network for secure international financial transactions.

- **MT messages** (legacy): Fixed-format messages (MT103 for customer transfers, MT202 for bank transfers, MT940 for statements)
- **MX messages** (ISO 20022): XML-based replacements for MT messages
- **SWIFT gpi**: Global payments innovation for end-to-end payment tracking, transparency, and speed
- **DDD mapping**: SWIFT messages map to domain events. An incoming MT103 corresponds to a PaymentReceived event; an outgoing MT103 corresponds to a PaymentInitiated event. ACLs translate between SWIFT formats and internal models.

### FIX Protocol (Financial Information eXchange)

The standard electronic messaging format for real-time securities trading communication.

- **Structure**: Tag-value pair messages (e.g., Tag 35=D for new order, Tag 35=8 for execution report)
- **Key message types**:
  - New Order Single (D) -- Submit a buy or sell order
  - Execution Report (8) -- Trade execution confirmation
  - Order Cancel Request (F) -- Request to cancel an order
  - Market Data Request (V) -- Subscribe to market data
- **FIX versions**: FIX 4.2, 4.4, 5.0, and FIXT (transport)
- **DDD mapping**: FIX messages map to Order and Trade entities. An incoming Execution Report creates a TradeExecuted domain event. ACLs translate between FIX tag-value formats and internal domain objects.

### XBRL (eXtensible Business Reporting Language)

The global standard for digital business and financial reporting.

- **Structure**: XML-based taxonomy with standardized element definitions for financial statement concepts
- **Taxonomies**: US GAAP, IFRS, Basel, Solvency II
- **Usage**: SEC EDGAR filings, European Securities and Markets Authority (ESMA) reporting, Basel regulatory returns
- **DDD mapping**: The Accounts & Ledger context produces data that maps to XBRL taxonomy elements. ACLs translate between internal ledger structures and XBRL instance documents.

### FpML (Financial products Markup Language)

The industry standard for over-the-counter (OTC) derivatives and structured products.

- **Structure**: XML schema covering interest rate, credit, equity, foreign exchange, and commodity derivatives
- **Key concepts**: Product definitions, trade confirmations, lifecycle events, valuations
- **DDD mapping**: FpML structures inform the design of derivative Trade entities and value objects in the Trading & Investments context

## Identifier Standards

### IBAN (International Bank Account Number)

- **Structure**: Up to 34 alphanumeric characters -- country code (2), check digits (2), BBAN (up to 30)
- **Validation**: MOD-97 check digit algorithm (ISO 7064)
- **DDD mapping**: IBAN value object with self-validating construction
- **Standard**: ISO 13616

### BIC (Bank Identifier Code)

- **Structure**: 8 or 11 characters -- institution (4), country (2), location (2), branch (3, optional)
- **Also known as**: SWIFT code
- **DDD mapping**: BIC value object paired with IBAN for international payment routing
- **Standard**: ISO 9362

### ISIN (International Securities Identification Number)

- **Structure**: 12 characters -- country code (2), national identifier (9), check digit (1)
- **DDD mapping**: SecurityIdentifier value object in the Trading & Investments context
- **Standard**: ISO 6166

### LEI (Legal Entity Identifier)

- **Structure**: 20 alphanumeric characters uniquely identifying legal entities in financial transactions
- **Usage**: Regulatory reporting, counterparty identification, derivatives trade reporting
- **Standard**: ISO 17442

## Security Standards

### PCI DSS (Payment Card Industry Data Security Standard)

- **Purpose**: Protect cardholder data during storage, processing, and transmission
- **Requirements**: Network security, access controls, encryption, monitoring, security testing
- **DDD impact**: Card payment data handling within the Payments context must comply; influences how value objects containing card data are designed and stored
- **Current version**: PCI DSS v4.0

## Payment Processing Standards

### NACHA / ACH (Automated Clearing House)

- **Structure**: Fixed-width batch file format with file header, batch header, entry detail, and control records
- **Transaction types**: Direct deposit (payroll), direct payment (bills), B2B payments
- **Processing**: Batch-processed through the Federal Reserve or EPN
- **DDD mapping**: ACL translates between internal Payment aggregates and NACHA file format. Each ACH entry corresponds to a Payment entity.
- **Governing body**: NACHA (National Automated Clearing House Association)

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate between external standards and internal domain models
- [Integration Patterns](../integration-patterns/) -- Standards define the published languages used in integration
- [Value Objects](../value-objects/) -- Identifier standards (IBAN, BIC, ISIN) are modeled as value objects
- [Payments & Transfers Context](../payments-transfers-context/) -- Primary consumer and producer of payment messaging standards
- [Trading & Investments Context](../trading-investments-context/) -- Primary consumer and producer of trading standards
- [Accounts & Ledger Context](../accounts-ledger-context/) -- Producer of XBRL financial reports
- [Ubiquitous Language](../ubiquitous-language/) -- Standards terminology informs the ubiquitous language

## References

- ISO. "ISO 20022: Universal Financial Industry Message Scheme." https://www.iso20022.org/
- SWIFT. "SWIFT Standards." https://www.swift.com/standards
- FIX Trading Community. "FIX Protocol Specification." https://www.fixtrading.org/
- XBRL International. "Extensible Business Reporting Language." https://www.xbrl.org/
- ISDA. "FpML: Financial products Markup Language." https://www.fpml.org/
- ISO 13616. "Financial Services -- International Bank Account Number (IBAN)." ISO, 2020.
- ISO 9362. "Banking -- Banking Telecommunication Messages -- Business Identifier Code (BIC)." ISO, 2019.
- ISO 6166. "Securities and Related Financial Instruments -- International Securities Identification Numbering System (ISIN)." ISO, 2021.
- PCI Security Standards Council. "PCI DSS v4.0." https://www.pcisecuritystandards.org/
- NACHA. "ACH Operating Rules." https://www.nacha.org/
