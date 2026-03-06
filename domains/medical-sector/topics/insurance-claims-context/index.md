# Insurance & Claims Context in Medical Practice

## Overview

The Insurance & Claims Context is the bounded context responsible for insurance coverage verification, prior authorization management, claims generation, claims submission, adjudication tracking, explanation of benefits (EOB) processing, denial management, and patient billing. This context translates clinical services into financial transactions and manages the complex interactions between medical practice and insurance payers.

## Core Responsibilities

- **Eligibility Verification**: Confirming patient insurance coverage before or at the time of service
- **Prior Authorization**: Obtaining payer approval for services, procedures, and medications that require pre-authorization
- **Claims Generation**: Transforming completed encounters into properly coded insurance claims with diagnosis and procedure codes
- **Claims Submission**: Transmitting claims to payers via EDI (837P for professional, 837I for institutional)
- **Adjudication Tracking**: Monitoring payer processing and recording payment, adjustment, and denial decisions
- **EOB Processing**: Interpreting explanation of benefits and remittance advice (835) from payers
- **Denial Management**: Tracking denied claims, identifying denial reasons, and managing the appeal process
- **Patient Billing**: Calculating patient responsibility (copay, deductible, coinsurance) and generating patient statements

## Aggregates

### Claim Aggregate

- **Aggregate Root**: Claim (identified by ClaimID)
- **Internal Entities**: ServiceLine (each service billed on the claim)
- **Value Objects**: DiagnosisCode (ICD-10-CM), ProcedureCode (CPT/HCPCS), PlaceOfService, BillingProvider (NPI, taxonomy), RenderingProvider, Modifier, ChargeAmount
- **Invariants**: A claim must have at least one service line; each service line must have a valid CPT code; each service line must link to at least one diagnosis code; the primary diagnosis must be present; total billed amount must equal sum of service line charges

### CoverageVerification Aggregate

- **Aggregate Root**: CoverageVerification (identified by VerificationID)
- **Value Objects**: CoverageStatus (eligible/ineligible, effective dates), BenefitDetail (copay, deductible remaining, coinsurance, out-of-pocket max), NetworkStatus (in-network/out-of-network)
- **Invariants**: Verification must reference a valid patient and insurance policy; coverage status must have an effective date

### PriorAuthorization Aggregate

- **Aggregate Root**: PriorAuthorization (identified by AuthorizationID)
- **Value Objects**: AuthorizedService (CPT code, quantity, date range), AuthorizationStatus (pending, approved, denied, expired), PayerReference
- **Invariants**: Authorized services cannot exceed the payer-approved quantity; expired authorizations cannot be used for claims

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| EligibilityVerified | Coverage confirmed or denied | Clinical Workflow, Scheduling |
| PriorAuthorizationRequested | Pre-auth submitted to payer | Clinical Workflow |
| PriorAuthorizationApproved | Payer grants approval | Clinical Workflow, Pharmacy |
| PriorAuthorizationDenied | Payer denies approval | Clinical Workflow (decision point) |
| ClaimGenerated | Claim created from encounter | Billing |
| ClaimSubmitted | Claim transmitted to payer | Audit, Finance |
| ClaimAdjudicated | Payer processes claim | Finance, Patient Billing |
| ClaimDenied | Payer denies claim | Denial Management |
| ClaimAppealed | Appeal submitted for denied claim | Audit |
| PatientStatementGenerated | Patient bill created | Patient notification |

## Domain Services

### ClaimGenerationService

Transforms a completed encounter into a properly coded claim by combining encounter diagnoses and procedures, patient demographics, insurance policy information, and provider billing details. Applies coding rules, modifier logic, and bundling/unbundling rules.

### EligibilityVerificationService

Queries external payer systems (via EDI 270/271 or payer APIs through ACL) to determine a patient's current coverage status, benefits, copay, and deductible information.

## Integration with Other Contexts

### Insurance <- Clinical Workflow (Customer-Supplier)

The Insurance context consumes EncounterCompleted events with diagnosis and procedure information to generate claims. It does not influence clinical decisions.

### Insurance <- Patient Records (Customer-Supplier)

The Insurance context consumes patient demographics and insurance policy data to populate claim fields and verify eligibility.

### Insurance <-> External Payers (Conformist)

Insurance payers dictate claim formats and protocols. The Insurance context must conform to:

- **837P/837I**: Professional and institutional claim submission formats
- **835**: Electronic remittance advice format
- **270/271**: Eligibility inquiry and response
- **278**: Prior authorization request and response

These are industry-mandated EDI standards under HIPAA.

## Coding and Billing Concepts

- **ICD-10-CM**: Diagnosis codes used to justify medical necessity for billed services
- **CPT/HCPCS**: Procedure codes describing the services performed
- **Modifiers**: Two-digit codes that provide additional information about a procedure (e.g., -25 for significant E/M service on same day as procedure)
- **Place of Service**: Code indicating where the service was rendered (11 = office, 02 = telehealth, 21 = inpatient)
- **Timely Filing**: Payers impose deadlines for claim submission (typically 90-365 days from date of service)

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Insurance & Claims is one of six medical bounded contexts
- [Clinical Workflow Context](../clinical-workflow-context/) -- Upstream source of encounter and procedure data
- [Patient Records Context](../patient-records-context/) -- Source of demographics and insurance policy data
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate EDI formats
- [Value Objects](../value-objects/) -- CPTCode, ICD10Code, CoverageStatus are key value objects
- [Domain Services](../domain-services/) -- ClaimGenerationService and EligibilityVerificationService
- [Regulatory Compliance](../regulatory-compliance/) -- HIPAA mandates EDI transaction standards

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- CMS. "Medicare Claims Processing Manual." https://www.cms.gov/
- AMA. "Current Procedural Terminology (CPT)." https://www.ama-assn.org/practice-management/cpt
- X12. "ASC X12 Health Care Transaction Standards." https://x12.org/
- WEDI. "Health Care EDI Standards Guide." https://www.wedi.org/
