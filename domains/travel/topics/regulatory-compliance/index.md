# Regulatory Compliance in Travel

## Overview

Regulatory compliance in the travel domain encompasses the legal and governance requirements that shape how travel systems handle personal data, process payments, protect passenger rights, enforce security, and manage cross-border travel documentation. These regulations are not optional business rules -- they are externally imposed constraints that must be reflected in the domain model design. Non-compliance carries significant financial penalties, legal liability, and reputational risk. In DDD terms, regulatory requirements often manifest as invariants on aggregates, constraints on value objects, and required domain events for audit trails.

## GDPR (General Data Protection Regulation)

The EU's General Data Protection Regulation (Regulation (EU) 2016/679) governs the collection, processing, storage, and transfer of personal data of EU residents.

### Impact on the Domain Model

- **Passenger Management Context**: Must support data subject rights including right of access (Article 15), right to rectification (Article 16), right to erasure (Article 17), and right to data portability (Article 20)
- **Consent management**: Processing of passenger data requires a lawful basis. Consent records must be maintained as domain objects with timestamp, scope, and withdrawal capability.
- **Data minimization**: Only collect and store passenger data that is necessary for the booking purpose. The Passenger Profile aggregate should not accumulate unnecessary attributes.
- **Retention policies**: Passenger data must be deleted or anonymized after the retention period expires. The domain model must support automated data lifecycle management.
- **Data breach notification**: Systems must detect and report personal data breaches within 72 hours (Article 33). Domain events should support breach detection workflows.
- **Cross-border transfers**: Passenger data transferred outside the EU requires appropriate safeguards (Standard Contractual Clauses, adequacy decisions).

### DDD Implications

- PassengerProfile aggregate includes consent tracking and data retention metadata
- Domain events like `ProfileArchived` and `DataDeletionRequested` support GDPR operations
- The Anti-Corruption Layer for GDS must ensure that personal data sent to third-party systems complies with transfer requirements

## PCI DSS (Payment Card Industry Data Security Standard)

PCI DSS v4.0 governs any system that stores, processes, or transmits payment card data.

### Impact on the Domain Model

- **Tokenization**: The Payment & Billing Context must never store raw credit card numbers. Payment methods are represented as tokenized value objects referencing data held by PCI-certified processors.
- **Network segmentation**: Payment processing components must be architecturally isolated from other bounded contexts.
- **Access logging**: All access to payment data must be logged with immutable audit records.
- **Encryption**: Payment data in transit must be encrypted (TLS 1.2+). Stored tokens must be protected.
- **Vulnerability management**: Payment-handling code requires regular security assessments.

### DDD Implications

- PaymentMethod value object holds only a token reference, never raw card data
- Payment aggregate maintains an audit trail of all operations via domain events
- The Payment & Billing Context is architecturally separate from other contexts, enforced at the bounded context level

## EU Passenger Rights (EC 261/2004)

European Parliament Regulation EC 261/2004 establishes common rules on compensation and assistance to air passengers in cases of denied boarding, cancellation, or long delay.

### Key Provisions

- **Denied boarding**: Passengers involuntarily denied boarding are entitled to compensation of EUR 250-600 depending on flight distance, plus re-routing or refund
- **Cancellation**: Passengers whose flights are cancelled are entitled to compensation (unless informed 14+ days in advance or extraordinary circumstances apply), plus re-routing or refund
- **Long delays**: Passengers delayed 2+ hours are entitled to meals and refreshments; 3+ hours at arrival triggers compensation; 5+ hours triggers refund option
- **Downgrading**: Passengers involuntarily downgraded are entitled to 30-75% fare reimbursement depending on flight distance

### DDD Implications

- The Reservation Context must track segment status changes (delays, cancellations) and passenger boarding status
- Domain events like `SegmentCancelled`, `SegmentDelayed`, and `BoardingDenied` trigger compensation eligibility assessment
- A CompensationService (domain service) evaluates eligibility based on flight distance, notification timing, and cause
- The Itinerary Context must record delay durations for compliance reporting

## TSA / Aviation Security Requirements

Transportation Security Administration (TSA) requirements and international equivalents impose security-related data requirements on travel systems.

### Key Requirements

- **Secure Flight**: Airlines must collect and transmit passenger data (full name, date of birth, gender) for TSA watchlist matching on US-bound flights
- **APIS (Advance Passenger Information System)**: Airlines must transmit passport and visa details to destination country authorities before departure
- **No Fly List / Selectee List**: Systems must support watchlist screening during booking and check-in
- **Known Traveler Number (KTN)**: TSA PreCheck and Global Entry numbers must be accepted and transmitted

### DDD Implications

- The Passenger Management Context must collect Secure Flight data fields (full legal name, DOB, gender, Redress Number, KTN)
- The Reservation Context must transmit APIS data to carriers before departure
- Special handling for passengers flagged during screening must be modeled without exposing security-sensitive logic to other contexts

## Visa and Passport Requirements

Cross-border travel requires validation of travel document adequacy for the destination and transit countries.

### Key Concerns

- **Passport validity**: Many countries require passports valid for 6 months beyond the travel date
- **Visa requirements**: Vary by passenger nationality, destination, purpose of travel, and length of stay
- **Transit visa requirements**: Some countries require visas even for airport transit
- **ESTA / eTA**: Electronic travel authorizations required for visa-waiver travelers to the US (ESTA) and Canada (eTA)

### DDD Implications

- The Passenger Management Context validates travel documents against destination requirements
- Document validation rules are modeled as domain services that reference country-specific requirement databases
- Domain events like `DocumentExpiring` and `VisaRequired` alert passengers and agents to documentation needs
- The Anti-Corruption Layer interfaces with TIMATIC or similar databases for real-time travel document validation

## Other Regulatory Considerations

- **Consumer protection laws**: Vary by jurisdiction; may require transparent pricing, right of withdrawal, and clear cancellation policies
- **Tax reporting**: Different jurisdictions require different tax reporting for travel transactions
- **Accessibility requirements**: ADA (US), EAA (EU) require accessible booking systems and passenger assistance
- **Data localization**: Some countries require passenger or payment data to be stored within national borders

## Relationships to Other Topics

- [Passenger Management Context](../passenger-management-context/) -- GDPR, Secure Flight, and document requirements shape this context
- [Payment & Billing Context](../payment-billing-context/) -- PCI DSS governs this context's architecture
- [Domain Events](../domain-events/) -- Regulatory events trigger compliance workflows
- [Value Objects](../value-objects/) -- Compliance constraints shape value object validation rules
- [Travel Standards](../travel-standards/) -- Industry standards overlap with regulatory requirements
- [Anti-Corruption Layer](../anti-corruption-layer/) -- TIMATIC and government systems accessed through ACLs

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- European Parliament. "Regulation (EU) 2016/679 (GDPR)." Official Journal of the European Union, 2016.
- European Parliament. "Regulation (EC) No 261/2004." Official Journal of the European Union, 2004.
- PCI Security Standards Council. "PCI DSS v4.0." 2022.
- TSA. "Secure Flight Program Documentation."
- IATA. "TIMATIC: Travel Information Manual Automatic."
- US Department of Homeland Security. "APIS: Advance Passenger Information System Requirements."
