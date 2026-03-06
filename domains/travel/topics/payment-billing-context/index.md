# Payment & Billing Context

## Overview

The Payment & Billing Context handles all financial operations in the travel domain: payment authorization and capture, payment method management, invoicing, refund processing, void operations, and financial settlement with airlines through IATA BSP (Billing and Settlement Plan). This context enforces PCI DSS compliance for credit card data handling and maintains strict separation between financial operations and booking logic. It treats payment as its own aggregate with its own lifecycle, independent of but coordinated with the Reservation Context.

## Key Domain Concepts

### Payment (Aggregate Root)

The central aggregate representing a financial transaction. A Payment is identified by a PaymentId and tracks the full lifecycle from authorization through capture, potential refund, or void. The Payment aggregate references a Reservation by ReservationId but does not contain reservation details -- it deals only with financial data.

### Payment Method

A value object representing the instrument used for payment: credit card (tokenized), debit card, bank transfer, travel agency credit, loyalty points, or corporate billing account. Credit card data is never stored in clear text -- only tokenized references from PCI-compliant payment processors.

### Authorization

The initial hold on a payment method confirming that funds are available. Authorization does not transfer funds -- it reserves them. Authorizations have a limited validity period (typically 7-30 days depending on the payment processor) and must be captured before expiration.

### Capture

The settlement of an authorized payment, transferring funds from the payer to the travel provider. Capture may occur immediately upon ticketing or be deferred until closer to the travel date. Partial capture is supported for bookings where only some segments are confirmed.

### Refund

The return of captured funds to the original payment method. Refund amounts are calculated by the Pricing & Fare Context based on fare rules, penalties, and unused coupons. The Payment Context executes the refund and records the transaction.

### Void

The cancellation of an authorization before capture occurs. Voids release the held funds immediately and are typically used within the first 24 hours of booking (the void window). No fees are charged for voided transactions.

### Invoice

A financial document generated for the booking, detailing base fares, taxes, fees, ancillary charges, and total amount. Invoices are issued to the traveler, travel agent, or corporate account and serve as the official financial record.

### BSP Remittance

The financial settlement between the travel agent (or platform) and airlines through IATA's Billing and Settlement Plan. BSP operates on a bi-weekly billing cycle. The Payment & Billing Context generates BSP-compliant transaction records for settlement.

## Invariants

- Capture amount cannot exceed the authorized amount
- Refund amount cannot exceed the total captured amount
- A voided payment cannot be captured or refunded
- All credit card data must be tokenized; raw card numbers never enter the domain model
- Payment method must be validated and authorized before any capture
- BSP remittance records must balance against issued tickets and refunds
- Multi-currency transactions must use consistent exchange rates within a single booking

## PCI DSS Compliance

The Payment & Billing Context operates under PCI DSS requirements:

- Credit card data is handled only by PCI-certified payment processors
- The domain model uses tokenized payment references, never raw card data
- All payment-related communication uses encrypted channels
- Access to payment data is logged and audited
- Payment processing components are isolated from other system components

## Integration Points

- **Reservation Context**: Receives payment triggers when bookings are confirmed or cancelled
- **Pricing & Fare Context**: Receives amounts, tax breakdowns, and refund calculations
- **Payment processors**: Stripe, Adyen, Worldpay, or airline-specific payment gateways via ACLs
- **IATA BSP**: Settlement reporting for airline ticket sales
- **Corporate billing systems**: Direct billing for corporate travel accounts
- **Accounting systems**: Financial reporting and reconciliation

## Domain Events

- **PaymentAuthorized**: Funds have been held on a payment method
- **PaymentCaptured**: Funds have been settled
- **PaymentFailed**: Authorization or capture failed
- **PaymentVoided**: Authorization cancelled before capture
- **RefundProcessed**: Refund issued to the original payment method
- **InvoiceGenerated**: Invoice created for the booking
- **BSPReportGenerated**: BSP settlement report produced for the billing cycle

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Payment is the aggregate root
- [Value Objects](../value-objects/) -- Currency, PaymentMethod, and PaymentReference are value objects
- [Domain Events](../domain-events/) -- Payment lifecycle events coordinate with other contexts
- [Reservation Context](../reservation-context/) -- Upstream trigger for payment operations
- [Pricing & Fare Context](../pricing-fare-context/) -- Provides amounts and refund calculations
- [Regulatory Compliance](../regulatory-compliance/) -- PCI DSS governs payment data handling
- [Travel Standards](../travel-standards/) -- IATA BSP governs airline settlement

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- PCI Security Standards Council. "PCI DSS v4.0: Payment Card Industry Data Security Standard."
- IATA. "BSP Manual for Agents."
- IATA. "Resolution 890: Standard Traffic Document Numbering."
