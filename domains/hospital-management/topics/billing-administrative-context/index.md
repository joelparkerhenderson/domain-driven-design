# Billing/Administrative Context in Hospital Management

## Overview

The Billing/Administrative Context manages the financial operations of the hospital: charge capture, medical coding, insurance claims, payment processing, denial management, and financial reporting. It translates clinical activities into billable events and manages the revenue cycle from service delivery to payment collection.

## Core Responsibilities

- **Charge Capture**: Recording billable services as they occur during clinical encounters
- **Medical Coding**: Translating clinical diagnoses and procedures into standardized billing codes (ICD-10, CPT)
- **Claims Management**: Generating, submitting, and tracking insurance claims
- **Payment Processing**: Recording payments from insurance companies and patients, managing accounts receivable
- **Denial Management**: Handling claim denials, appeals, and resubmissions
- **Financial Reporting**: Producing revenue reports, aging reports, and payer performance analysis

## Aggregates

### Claim Aggregate

- **Root**: Claim (identified by ClaimID)
- **Internal entities**: ClaimLineItem
- **Value objects**: BillingCode (CPT), DiagnosisCode (ICD-10), Money, PayerInfo, ServiceDate
- **States**: Draft → Submitted → Adjudicated → Paid / Denied / Partially Paid
- **Invariants**:
  - Every line item must have a valid CPT code and at least one supporting ICD-10 diagnosis
  - Claim total equals sum of line item amounts
  - Submitted claims are immutable; corrections require new adjustment claims
  - Timely filing deadlines must be met per payer contract

### Account Aggregate

- **Root**: Account (identified by AccountID, linked to Patient MRN)
- **Internal entities**: ChargeItem, PaymentRecord
- **Value objects**: Money, DateRange, AccountStatus
- **Invariants**:
  - Account balance equals charges minus payments and adjustments
  - Patient responsibility calculated after insurance adjudication
  - Write-offs require authorization

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| ChargeRecorded | Billable service performed | Coding queue |
| ClaimGenerated | Charges coded and bundled | Claims submission |
| ClaimSubmitted | Claim sent to payer | Claims tracking |
| ClaimAdjudicated | Payer processes claim | Account update |
| PaymentReceived | Payment posted | Account update, financial reporting |
| ClaimDenied | Payer denies claim | Denial management queue |
| PatientStatementGenerated | Patient balance due | Patient notification |

## Integration with Other Contexts

### Clinical/EMR → Billing (Customer-Supplier with Anti-Corruption Layer)

The Billing context is the downstream consumer of clinical events. An anti-corruption layer translates clinical concepts:

- EncounterCompleted with diagnoses → ClaimLineItems with ICD-10 codes
- OrderPlaced (procedure) → ChargeItem with CPT code
- Provider NPI → Rendering/billing provider on claim

The ACL handles the semantic gap between "clinical diagnosis" and "billable diagnosis code."

### Patient → Billing (Customer-Supplier)

Insurance policy information flows from the Patient context. Billing maintains its own AccountHolder and CoverageDetails projections.

### Billing → External Insurance (Anti-Corruption Layer)

Insurance companies use EDI formats (837P/837I for claims, 835 for remittance). The ACL:

- Translates domain Claim objects into EDI 837 format for submission
- Parses EDI 835 remittance advice into domain PaymentReceived events
- Maps proprietary denial codes to internal DenialReason value objects

## Medical Coding Standards

### ICD-10 (Diagnoses)

International Classification of Diseases, 10th Revision. Used to code diagnoses on claims.

- Example: J18.9 — Pneumonia, unspecified organism
- Example: I10 — Essential (primary) hypertension

### CPT (Procedures)

Current Procedural Terminology. Used to code medical services and procedures.

- Example: 99213 — Office visit, established patient, low complexity
- Example: 71046 — Chest X-ray, 2 views

### HCPCS (Supplies and Equipment)

Healthcare Common Procedure Coding System. Extends CPT for non-physician services.

### Revenue Codes

Facility-specific codes on institutional claims (UB-04) identifying department or type of service.

## Key Workflows

### Revenue Cycle

1. **Service Delivery**: Clinical encounter occurs, charges captured
2. **Coding**: Coders review documentation, assign ICD-10 and CPT codes
3. **Claim Generation**: System bundles coded charges into claims
4. **Submission**: Claims sent electronically to payers
5. **Adjudication**: Payer processes claim, determines payment
6. **Posting**: Payments and adjustments recorded
7. **Patient Billing**: Remaining balance billed to patient
8. **Collections**: Overdue balances sent to collections

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of five hospital bounded contexts
- [Anti-Corruption Layer](../anti-corruption-layer/) — Critical for insurance API integration
- [Context Map](../context-map/) — Downstream consumer of Clinical and Patient contexts
- [Healthcare Standards](../healthcare-standards/) — ICD-10, CPT, HCPCS are central to billing
- [Domain Events](../domain-events/) — Clinical events trigger charge capture and claim generation
- [Value Objects](../value-objects/) — Money, BillingCode, DiagnosisCode are core value objects

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 15. Wrox, 2015.
- American Medical Association. "CPT Professional Edition." AMA, published annually.
- World Health Organization. "ICD-10: International Statistical Classification of Diseases." WHO, 2019.
- CMS. "Medicare Claims Processing Manual." Centers for Medicare & Medicaid Services.
- WEDI. "Health Care EDI Transaction Standards Guide." Workgroup for Electronic Data Interchange.
