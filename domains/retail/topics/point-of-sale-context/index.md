# Point of Sale Context

## Overview

The Point of Sale (POS) Context is the bounded context responsible for transaction processing, tender handling, register operations, and cashier workflows in retail stores. The POS context is where the sale happens: items are scanned, prices are calculated, discounts are applied, taxes are computed, payment is collected, and receipts are generated. This context demands high reliability, fast response times, and the ability to operate even when network connectivity to central systems is degraded.

## Core Concepts

**Transaction**: The central concept representing a retail sale. A transaction contains line items (products being purchased), applied discounts, calculated taxes, and tenders (payments). Transactions have a lifecycle: initiated, items added, subtotaled, tendered, completed (or voided). Each transaction is uniquely identified by a combination of store number, register number, date, and sequence number.

**Line Item**: An entry on a transaction representing a product being purchased. A line item includes the product identifier, description, quantity, unit price, extended price (quantity times unit price), and any item-level discounts. Line items are created when products are scanned or manually entered.

**Tender**: A payment applied to a transaction. Tender types include cash, credit card, debit card, gift card, mobile payment (Apple Pay, Google Pay), loyalty points, and store credit. Each tender has an amount and may have associated authorization data (for card payments). The total of all tenders must equal or exceed the transaction total.

**Register**: A physical or virtual point-of-sale terminal. Registers track their operational state (open, closed, suspended), the currently signed-on cashier, and the current transaction. Register operations include opening (with a starting cash float), closing (with end-of-day reconciliation), and cash drops.

**Receipt**: The record of a completed transaction provided to the customer. Receipts include line items, discounts, subtotal, taxes, total, tenders, and change due. Receipts may also include loyalty points earned, return policy, and promotional messages.

**Return and Exchange**: The process of accepting previously purchased merchandise back and issuing a refund or exchange. Returns require validation against the original transaction, enforcement of return policies (time limits, receipt requirements), and appropriate refund tender determination.

## Aggregates

The **Transaction Aggregate** is the primary aggregate, rooted at the Transaction entity and containing LineItem entities, TenderAmount value objects, TaxAmount value objects, and discount records. Key invariants: the sum of tenders must cover the transaction total, voided items must be properly reflected in totals, and tax calculations must be consistent with the selling jurisdiction.

The **Register Aggregate** manages register state. Invariants: only one cashier can be signed on at a time, a register must be opened before processing transactions, and cash drawer counts must reconcile at close.

## Domain Events

- TransactionCompleted: Published when a sale is fully tendered and completed.
- TransactionVoided: Published when a transaction is cancelled.
- ReturnProcessed: Published when a return is completed with refund issued.
- RegisterOpened: Published when a register begins operations.
- RegisterClosed: Published when a register ends operations and is reconciled.

## Domain Services

**TaxCalculationService**: Calculates taxes for each line item based on the store's tax jurisdiction, product tax categories, and applicable exemptions. Multi-jurisdiction scenarios (such as online orders shipping across state lines) add complexity.

**TenderValidationService**: Validates that tenders cover the transaction total, enforces tender-specific rules (split tender limits, cash-back limits), and determines change due.

## Integration with Other Contexts

- **Pricing and Promotions Context** (upstream): POS requests price lookups and promotion evaluations during transaction building. The POS does not own prices; it consumes them.
- **Customer and Loyalty Context**: POS identifies the customer during checkout, applies member discounts, and reports completed transactions for loyalty point accrual.
- **Inventory and Merchandising Context**: POS transactions generate inventory decrements. The POS may validate stock availability during scanning.
- **Product Catalog Context** (upstream): POS uses product data for item lookup and receipt description.
- **Omnichannel Context**: POS processes pickups for BOPIS orders and handles returns for online purchases.

## Offline Capability

The POS context must support offline operation. When network connectivity is lost, the POS must continue processing transactions using locally cached product data, prices, and promotion rules. Transactions completed offline are queued and synchronized when connectivity is restored. This requirement shapes the architecture significantly, requiring local data stores, conflict resolution strategies, and eventual consistency handling.

## PCI DSS Considerations

The POS context handles payment card data and must comply with PCI DSS requirements. Card data is handled through point-to-point encryption (P2PE) devices that prevent the POS software from accessing raw card numbers. Tokenization replaces card data with tokens for transaction records. The POS domain model should never contain raw card numbers as entities or value objects.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): POS is a core retail bounded context.
- [Aggregates](../aggregates/index.md): Transaction and Register are the primary aggregates.
- [Entities](../entities/index.md): Transaction, Register, and Line Item are key entities.
- [Value Objects](../value-objects/index.md): TenderAmount, TaxAmount, and ReceiptLine are value objects.
- [Pricing Promotions Context](../pricing-promotions-context/index.md): POS consumes pricing services.
- [Regulatory Compliance](../regulatory-compliance/index.md): PCI DSS compliance for payment handling.
- [Retail Standards](../retail-standards/index.md): ARTS standards for POS data models.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- PCI Security Standards Council. *PCI Data Security Standard (PCI DSS)*. Version 4.0, 2022.
- ARTS (Association for Retail Technology Standards). *ARTS Unified POS Data Model*. NRF, ongoing.
