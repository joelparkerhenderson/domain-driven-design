# Anti-Corruption Layer in Finance

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context's domain model from the concepts, terminology, and data structures of external systems. The ACL translates between the external model and the internal model, preventing foreign concepts from leaking into and corrupting the domain.

## Relevance to Finance

Financial systems integrate with numerous external systems: payment networks (SWIFT, Fedwire), market data providers (Bloomberg, Reuters), regulatory reporting systems, credit bureaus, legacy core banking platforms, and third-party fintech services. Each of these systems has its own data model and vocabulary. Without anti-corruption layers, the internal domain model would become a tangled mix of external system concepts.

## Key Concepts

### Translation

The ACL translates external data formats, terminology, and structures into the internal ubiquitous language. For example:

- A SWIFT MT103 message with fields like `:32A:` (value date and amount) is translated into internal `Payment` entities with `settlementDate` and `Money` value objects
- A credit bureau response with a FICO score is translated into an internal `CreditScore` value object with institution-specific risk interpretation
- An ISO 20022 `pain.001` payment initiation message is translated into the internal `PaymentOrder` aggregate

### Isolation

The ACL ensures that if an external system changes its API, data format, or terminology, the impact is contained within the translation layer. The internal domain model remains stable.

### Adapter Pattern

The ACL is typically implemented using the adapter pattern: an interface defined in the domain's terms, with an implementation that handles the translation from external system formats.

## Finance-Specific Anti-Corruption Layers

### Legacy Core Banking ACL

Many financial institutions operate legacy core banking systems that use flat files, COBOL copybooks, or proprietary message formats. The ACL translates between legacy formats and the modern domain model.

- **External model**: Fixed-width records, numeric account codes, packed decimal amounts
- **Internal model**: Rich domain entities with Money value objects, AccountNumber, and typed identifiers
- **Translation**: Legacy field mappings, character encoding conversion, decimal precision normalization

### Payment Network ACL

Payment networks such as SWIFT, ACH/NACHA, and Fedwire each define their own message standards.

- **SWIFT**: MT (legacy) and MX (ISO 20022) message formats translated into internal PaymentOrder and Settlement models
- **ACH/NACHA**: Batch file format with fixed-width records translated into internal Payment entities
- **Real-time payments**: API-based formats (e.g., FedNow, RTP) translated into internal PaymentOrder with immediate settlement semantics

### Market Data ACL

Market data providers deliver pricing and reference data in proprietary formats.

- **External model**: Vendor-specific ticker symbols, proprietary data schemas, FIX protocol messages
- **Internal model**: SecurityMaster entities, MarketQuote value objects, standardized instrument identifiers (ISIN, CUSIP)
- **Translation**: Ticker symbol mapping, price normalization, timestamp synchronization

### Regulatory Reporting ACL

Regulatory bodies require submissions in specific formats.

- **External model**: XBRL taxonomies, Basel reporting templates, FinCEN CTR/SAR formats
- **Internal model**: Domain aggregates and events that capture the business reality
- **Translation**: Domain data mapped to regulatory taxonomy elements and report formats

## Design Guidelines

1. **One ACL per external system**: Each external integration gets its own ACL to contain translation complexity
2. **Domain interface, infrastructure implementation**: Define the ACL interface using domain concepts; implement the translation in the infrastructure layer
3. **Fail-safe translation**: Handle unexpected external data gracefully with validation and error reporting
4. **Version management**: External system APIs change; the ACL must manage version differences without impacting the domain model

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- ACLs protect bounded context boundaries
- [Integration Patterns](../integration-patterns/) -- ACLs are a key integration pattern
- [Context Map](../context-map/) -- ACLs appear on the context map at integration points
- [Finance Standards](../finance-standards/) -- ACLs translate between external standards and internal models
- [Value Objects](../value-objects/) -- ACLs construct internal value objects from external data

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 7: Context Mapping. Wrox, 2015.
- SWIFT. "SWIFT Standards." https://www.swift.com/standards
