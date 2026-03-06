# Integration Patterns in Finance

## Overview

Integration patterns define how bounded contexts communicate and share data while maintaining their autonomy and model integrity. Domain-Driven Design identifies several standard integration patterns that describe the relationship dynamics, data flow mechanisms, and boundary protection strategies between contexts.

## Relevance to Finance

Financial institutions operate dozens of interconnected systems: core banking, trading platforms, risk engines, payment networks, regulatory reporting systems, and third-party services. Choosing the right integration pattern for each relationship determines how tightly or loosely coupled contexts become, how changes propagate, and how failures are isolated.

## DDD Integration Patterns in Finance

### Published Language

A well-documented, shared data format used for communication between contexts. Both sides agree to the language, which is versioned and maintained independently.

**Finance applications**:

- **ISO 20022** as the published language for payment messages between Payments & Transfers and external payment networks
- **FIX protocol** as the published language for order and execution messages between Trading and external brokers/exchanges
- **XBRL** as the published language for financial reporting between Accounts & Ledger and regulatory bodies
- **CustomerOnboarded event schema** as the published language for customer identity data shared across all contexts

### Open Host Service

A context exposes a well-defined API (the "open host") that other contexts can consume. The API uses the published language and is designed for general consumption.

**Finance applications**:

- Risk & Compliance exposes a screening API that Payments, Trading, and Lending contexts call for fraud checks, sanctions screening, and risk assessment
- Customer Onboarding exposes a customer verification API used during loan applications and account opening
- Market data services expose pricing APIs consumed by Trading, Risk, and Accounts & Ledger contexts

### Customer-Supplier

An upstream context (supplier) provides data or services that a downstream context (customer) depends on. The supplier commits to meeting the customer's needs.

**Finance applications**:

- Payments & Transfers (supplier) -> Accounts & Ledger (customer): Payment settlement data drives journal entry creation
- Lending & Credit (supplier) -> Accounts & Ledger (customer): Loan events drive ledger postings
- Trading & Investments (supplier) -> Accounts & Ledger (customer): Trade settlement data drives securities and cash journal entries

### Conformist

The downstream context conforms to the upstream context's model without the upstream making any accommodations. The downstream context adopts the upstream's language and data structures.

**Finance applications**:

- Payments & Transfers conforms to Risk & Compliance screening rules -- payments must pass compliance checks without negotiating the rules
- Trading & Investments conforms to Risk & Compliance position limits and pre-trade checks
- All contexts conform to regulatory reporting requirements defined by external regulatory bodies

### Anti-Corruption Layer

A translation layer that protects a context's internal model from external system concepts. The ACL translates between the foreign model and the internal domain model.

**Finance applications**:

- ACL between internal payment model and SWIFT/ACH message formats
- ACL between internal trade model and exchange-specific FIX message variations
- ACL between internal customer model and external credit bureau response formats
- ACL between internal ledger model and legacy core banking system data structures

### Shared Kernel

A small, explicitly shared subset of the domain model maintained jointly by two or more contexts. Changes require coordination between all sharing teams.

**Finance applications**:

- Shared Money value object (amount + currency) used across all contexts
- Shared CustomerID identifier used to correlate customer data across contexts
- Shared SecurityIdentifier (ISIN, CUSIP) used by Trading and Accounts & Ledger

**Caution**: Shared kernels create coupling. In finance, they should be limited to truly universal concepts like Money and identifier types.

### Partnership

Two contexts that coordinate closely, with both teams jointly planning and evolving their integration. Neither is upstream or downstream; they are peers.

**Finance applications**:

- Lending & Credit and Risk & Compliance operate in partnership for credit underwriting -- both teams must coordinate changes to risk assessment workflows
- Trading & Investments and Risk & Compliance partner on pre-trade risk checks

### Separate Ways

Two contexts that have no integration. They operate independently and do not share data.

**Finance applications**:

- Internal HR systems and the Trading context have no need for integration
- Marketing analytics and the General Ledger operate independently

## Choosing the Right Pattern

| Consideration | Recommended Pattern |
|--------------|-------------------|
| Standardized external communication | Published Language |
| General-purpose API for multiple consumers | Open Host Service |
| Clear upstream/downstream dependency | Customer-Supplier |
| Must accept external rules without negotiation | Conformist |
| Protecting internal model from external formats | Anti-Corruption Layer |
| Small, universal concepts shared across contexts | Shared Kernel |
| Close coordination between peer teams | Partnership |
| No relationship needed | Separate Ways |

## Relationships to Other Topics

- [Context Map](../context-map/) -- Integration patterns define the relationships shown on the context map
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Detailed coverage of the ACL pattern
- [Event-Driven Architecture](../event-driven-architecture/) -- Events are the primary mechanism for many integration patterns
- [Bounded Contexts](../bounded-contexts/) -- Integration patterns connect bounded contexts
- [Finance Standards](../finance-standards/) -- Standards serve as published languages for inter-context communication

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 7: Context Mapping. Wrox, 2015.
