# Payments & Transfers Context

## Overview

The Payments & Transfers context manages the movement of money between parties. It handles payment initiation, routing, authorization, clearing, settlement, and reconciliation across multiple payment rails including wire transfers, ACH batch processing, real-time payment networks, and card networks. This context is responsible for ensuring that money moves accurately, securely, and in compliance with regulatory requirements.

## Core Concepts

### Payment Order

A payment order is an instruction to transfer a specific amount of money from a payer to a payee. It includes the amount, currency, source account, destination account, payment method, and any required references or remittance information.

### Payment Rails

Payment rails are the networks and protocols through which payments travel:

- **Wire transfers**: High-value, same-day settlement through Fedwire (domestic) or SWIFT (international)
- **ACH (Automated Clearing House)**: Batch-processed payments for payroll, direct deposits, and bill payments via the NACHA network
- **Real-time payments**: Instant settlement through networks like FedNow, RTP (The Clearing House), and SEPA Instant (Europe)
- **Card networks**: Credit and debit card transactions through Visa, Mastercard, and similar networks
- **SWIFT gpi**: Global payments initiative for transparent, fast cross-border payments with end-to-end tracking

### Clearing and Settlement

Clearing is the process of reconciling and matching payment instructions between counterparties. Settlement is the final, irrevocable transfer of funds. These may happen simultaneously (real-time gross settlement) or in batches (net settlement).

### Correspondent Banking

For international payments, correspondent banking relationships enable money to flow between institutions that do not have direct accounts with each other. Payments route through intermediary banks, each adding processing time and fees.

- **Nostro account**: The sending bank's account at the foreign correspondent bank
- **Vostro account**: The foreign bank's account at the domestic bank (mirror of nostro)

## Aggregates in This Context

### Payment Aggregate

- **Root**: Payment (PaymentID)
- **Contains**: PaymentInstruction, ClearingRecord, SettlementRecord
- **Value objects**: Money, IBAN, BIC, PaymentMethod, PaymentStatus
- **Key invariant**: Payment amount must be positive; settled payments are immutable

### TransferBatch Aggregate

- **Root**: TransferBatch (BatchID)
- **Contains**: BatchItem list
- **Value objects**: BatchStatus, ProcessingDate
- **Key invariant**: Batch totals must equal the sum of individual items; batches cannot be modified after submission

## Domain Events

- PaymentInitiated -- payment order created and submitted for processing
- PaymentAuthorized -- payment passed fraud screening, sanctions checks, and balance validation
- PaymentCleared -- payment matched and reconciled through the clearing network
- PaymentSettled -- funds irrevocably transferred; consumed by Accounts & Ledger for journal posting
- PaymentRejected -- payment failed due to insufficient funds, sanctions hit, or network error
- PaymentReversed -- previously settled payment reversed via chargeback or return

## Integration with Other Contexts

- **Accounts & Ledger** (downstream): PaymentSettled events trigger journal entries
- **Risk & Compliance** (upstream): Payments must pass fraud screening and sanctions checks before authorization
- **Customer Onboarding** (upstream): Customer identity and account data for payment validation
- **Lending & Credit**: Loan disbursements and repayments flow through the payment network

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- One of the six bounded contexts in the finance domain
- [Context Map](../context-map/) -- Customer-supplier relationship with Accounts & Ledger; conformist to Risk & Compliance
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate between internal model and external payment network formats (SWIFT, ACH, FedNow)
- [Finance Standards](../finance-standards/) -- ISO 20022, SWIFT, NACHA/ACH, and PCI DSS are central to this context
- [Regulatory Compliance](../regulatory-compliance/) -- PSD2 (Europe), Regulation E (US), and sanctions screening

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- NACHA. "ACH Operating Rules." https://www.nacha.org/
- SWIFT. "SWIFT Standards." https://www.swift.com/standards
- ISO. "ISO 20022: Universal Financial Industry Message Scheme." https://www.iso20022.org/
- The Clearing House. "RTP Network." https://www.theclearinghouse.org/payment-systems/rtp
- Federal Reserve. "FedNow Service." https://www.frbservices.org/financial-services/fednow
