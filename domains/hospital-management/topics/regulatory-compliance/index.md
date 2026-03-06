# Regulatory Compliance in Hospital Management

## Overview

Regulatory compliance in healthcare imposes strict requirements on how patient data is stored, accessed, shared, and audited. These regulations directly shape the design of domain models, aggregates, events, and access control mechanisms. In a DDD hospital system, compliance is not an afterthought — it is a domain concern that influences aggregate boundaries, event design, and cross-cutting infrastructure.

## Relevance to Hospital Management

Hospital systems handle some of the most sensitive personal data. Regulatory violations result in:

- Federal and state fines (up to $1.9 million per violation category per year under HIPAA)
- Criminal penalties for willful violations
- Loss of Medicare/Medicaid certification
- Reputational damage and loss of patient trust
- Mandatory breach notification to affected individuals and HHS

Compliance requirements must be embedded in the domain model, not layered on as an afterthought.

## HIPAA (Health Insurance Portability and Accountability Act)

### Privacy Rule (45 CFR Part 164, Subpart E)

Governs the use and disclosure of Protected Health Information (PHI).

**DDD implications**:

- **Minimum Necessary Standard**: Domain queries and events should carry only the minimum PHI needed for the specific purpose. A billing event does not need the patient's full clinical history.
- **Consent as a Domain Concept**: Patient consent is modeled as a first-class domain object. The Patient aggregate tracks consent grants and revocations.
- **Accounting of Disclosures**: Every disclosure of PHI to external parties must be logged. Domain events (e.g., PatientRecordDisclosed) support this requirement.
- **Patient Access Rights**: Patients have the right to access their records. The system must support generating a complete patient record on request.

### Security Rule (45 CFR Part 164, Subpart C)

Requires administrative, physical, and technical safeguards for electronic PHI (ePHI).

**DDD implications**:

- **Access Control**: Role-based access control is enforced at the bounded context level. A billing clerk cannot access clinical notes; a nurse cannot access billing details.
- **Audit Trails**: Every access to and modification of ePHI must be logged. Domain events naturally support this — every state change produces an immutable event record.
- **Data Integrity**: Aggregates enforce data integrity through invariant checks. Event sourcing provides an immutable audit trail of all changes.
- **Encryption**: PHI at rest and in transit must be encrypted. This is an infrastructure concern but affects how domain events are serialized and stored.

### Breach Notification Rule

Requires notification of affected individuals, HHS, and media (for large breaches) when unsecured PHI is compromised.

**DDD implications**: Breach detection and notification can be modeled as a domain process — a BreachInvestigation aggregate that tracks affected records, notification status, and remediation actions.

## EMTALA (Emergency Medical Treatment and Labor Act)

Requires hospitals with emergency departments to provide medical screening and stabilization to anyone regardless of ability to pay.

**DDD implications**:

- The Emergency Services Context must not require insurance verification before triage and treatment
- EmergencyEncounter creation cannot be blocked by billing or insurance status
- Disposition decisions (admit/transfer/discharge) must be based on clinical criteria, not financial criteria

## Joint Commission Standards

The Joint Commission accredits hospitals and sets standards for patient safety, care quality, and organizational management.

**DDD implications**:

- **Medication Management**: Drug interaction checking, allergy verification, and dosage validation are required. These map to domain services (DrugInteractionChecker) and aggregate invariants.
- **Patient Identification**: Two-patient-identifier rule for all clinical activities. The Patient entity must support multiple identifiers (MRN, name+DOB, wristband).
- **Handoff Communication**: Structured communication during care transitions (SBAR format). Modeled as value objects within transfer and discharge events.

## State Regulations

State-specific requirements that supplement federal regulations:

- **State breach notification laws**: Often more stringent than federal HIPAA requirements
- **Mental health and substance abuse privacy**: 42 CFR Part 2 restricts disclosure of substance abuse treatment records more strictly than HIPAA
- **Minor consent laws**: State-specific rules on when minors can consent to treatment without parental involvement
- **Reporting requirements**: Mandatory reporting of communicable diseases, abuse, gunshot wounds

**DDD implications**: State-specific rules may require conditional logic in domain services and additional domain events (e.g., MandatoryReportFiled).

## Compliance in Domain Design

### Audit Trail as Domain Events

Every clinically or legally significant action produces a domain event that is stored immutably:

- PatientRecordAccessed (who, when, what was accessed, purpose)
- PatientRecordModified (who, when, what changed, previous value)
- PatientRecordDisclosed (to whom, what was disclosed, legal basis)
- ConsentGranted / ConsentRevoked (patient, scope, effective date)

These events form a complete, tamper-proof audit trail that satisfies HIPAA Security Rule requirements.

### Consent as an Aggregate

Consent management is complex enough to warrant its own aggregate within the Patient Context:

- **ConsentRecord** (aggregate root): Patient MRN, consent type, scope, status, effective period
- **ConsentTypes**: Treatment, data sharing, research participation, marketing
- **Invariants**: Active consent required before non-emergency data sharing; consent cannot be backdated
- **Events**: ConsentGranted, ConsentRevoked, ConsentExpired

### Access Control at Context Boundaries

Each bounded context enforces access policies at its API boundary:

- Clinical/EMR: Only authenticated providers with active credentials
- Billing: Only authorized billing staff and financial officers
- Patient: Only registration staff and the patients themselves (via patient portal)
- Emergency: Broader access for clinical staff during emergency care (break-the-glass with logging)

### Break-the-Glass

Emergency situations may require accessing data outside normal authorization. Break-the-glass provides this access while creating enhanced audit records:

- Access is granted immediately
- A BreakTheGlassAccessEvent is recorded with the clinician's justification
- Compliance team reviews all break-the-glass events

## Relationships to Other Topics

- [Domain Events](../domain-events/) — Events form the audit trail backbone
- [Aggregates](../aggregates/) — Compliance requirements shape aggregate boundaries and invariants
- [Patient Context](../patient-context/) — Consent management and patient access rights
- [Emergency Services Context](../emergency-services-context/) — EMTALA compliance
- [Billing/Administrative Context](../billing-administrative-context/) — Claims compliance, coding accuracy
- [Event-Driven Architecture](../event-driven-architecture/) — Event sourcing supports audit requirements

## References

- U.S. Department of Health and Human Services. "HIPAA Privacy Rule." 45 CFR Part 164, Subpart E. https://www.hhs.gov/hipaa/
- U.S. Department of Health and Human Services. "HIPAA Security Rule." 45 CFR Part 164, Subpart C.
- U.S. Department of Health and Human Services. "Breach Notification Rule." 45 CFR Part 164, Subpart D.
- Centers for Medicare & Medicaid Services. "EMTALA." 42 U.S.C. § 1395dd.
- The Joint Commission. "Hospital Accreditation Standards." Published annually.
- SAMHSA. "42 CFR Part 2: Confidentiality of Substance Use Disorder Patient Records." 2020.
- ONC. "Health IT Certification Criteria." 45 CFR Part 170.
- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
