# Regulatory Compliance in Medical Practice

## Overview

Regulatory compliance in medical practice encompasses the legal, governance, and industry requirements that shape how patient data is collected, stored, transmitted, and used. In Domain-Driven Design, compliance requirements are not afterthoughts -- they are first-class domain concerns that influence aggregate design, event modeling, integration patterns, and cross-cutting infrastructure. A medical practice system must comply with HIPAA, HITECH, FDA regulations, state medical laws, DEA controlled substance rules, and international data protection laws such as GDPR for health data.

## Key Regulations

### HIPAA (Health Insurance Portability and Accountability Act)

- **Privacy Rule** (45 CFR Part 160, 164 Subparts A, E): Governs who can access protected health information (PHI), under what circumstances, and what patient rights exist (access, amendment, accounting of disclosures)
- **Security Rule** (45 CFR Part 164 Subparts A, C): Requires administrative, physical, and technical safeguards for electronic PHI (ePHI), including access controls, audit logging, encryption, and integrity controls
- **Transaction Rule** (45 CFR Part 162): Mandates specific EDI transaction formats (837, 835, 270/271, 278) for electronic healthcare transactions
- **Breach Notification Rule** (45 CFR Part 164 Subpart D): Requires notification to affected individuals, HHS, and in some cases the media when a breach of unsecured PHI occurs

DDD impact: Every bounded context that handles PHI must implement access controls, audit logging, and minimum necessary data exposure. Domain events carrying PHI must be encrypted in transit and at rest. The Privacy Rule's "minimum necessary" principle directly influences what data is included in cross-context event payloads.

### HITECH Act (Health Information Technology for Economic and Clinical Health)

- **Purpose**: Strengthens HIPAA enforcement, promotes meaningful use of EHRs, and extends HIPAA requirements to business associates
- **Key Provisions**: Increased penalties for HIPAA violations, breach notification requirements, requirements for certified EHR technology, patient right to electronic copies of health records

DDD impact: HITECH reinforces the need for comprehensive audit trails (supported by domain events), data portability (supported by published languages like FHIR), and business associate management (tracked as part of the integration pattern with external systems).

### FDA 21 CFR Part 11

- **Purpose**: Governs electronic records and electronic signatures in FDA-regulated environments
- **Key Requirements**: Validation of systems, audit trails, record retention, access controls, electronic signature controls, authority checks
- **Applicability**: Applies when a medical system is used in clinical trials, pharmaceutical manufacturing, or other FDA-regulated activities

DDD impact: Aggregates in FDA-regulated contexts must support complete audit trails with tamper-evident records. Event sourcing is a natural fit because it provides an immutable, append-only log of all changes. Electronic signatures require domain-level modeling of signer identity, intent, and timestamp.

### GDPR Health Data Provisions

- **Purpose**: The EU General Data Protection Regulation classifies health data as a "special category" requiring additional protections
- **Key Requirements**: Explicit consent for health data processing, right to erasure (with medical record retention exceptions), data protection impact assessments, data portability, mandatory data protection officer
- **Applicability**: Applies when treating EU residents or when processing health data of individuals in the EU

DDD impact: The right to erasure conflicts with medical record retention requirements, requiring domain-level rules for what can be deleted versus what must be anonymized. Consent management becomes a first-class domain concept modeled in the Patient Records context.

### State Medical Regulations

- **Scope**: Each U.S. state has its own medical practice laws governing licensure, telemedicine, prescribing, consent, and record retention
- **Key Variations**: Telemedicine prescribing rules, controlled substance prescription requirements, medical record retention periods (typically 7-10 years for adults, until age 21 for minors), informed consent requirements, mandatory reporting obligations

DDD impact: State-specific rules must be modeled as configurable domain rules rather than hardcoded logic. The Telemedicine context must verify provider licensure in the patient's state. The Pharmacy context must enforce state-specific controlled substance rules.

### DEA Controlled Substance Regulations

- **Purpose**: The Drug Enforcement Administration regulates the prescribing, dispensing, and tracking of controlled substances (Schedules I-V)
- **Key Requirements**: Valid DEA registration for prescribers, prescription limits by schedule (no refills for Schedule II), EPCS (Electronic Prescribing for Controlled Substances) requirements, PDMP (Prescription Drug Monitoring Program) reporting

DDD impact: The Pharmacy & Medication context must enforce DEA rules as aggregate invariants. DEA number validation, schedule-specific refill limits, and PDMP reporting are domain rules, not infrastructure concerns. ControlledSubstancePrescribed domain events support PDMP integration.

## Compliance as Domain Logic

Regulatory compliance is not a cross-cutting infrastructure concern layered on top of the domain -- it is part of the domain model itself:

- **Access control rules** are domain policies that determine who can view, modify, or share patient data
- **Audit trail requirements** are satisfied naturally by domain events, which record what happened, when, and by whom
- **Consent management** is modeled as entities and value objects in the Patient Records context
- **Minimum necessary principle** shapes what data is included in domain event payloads and cross-context projections
- **Retention rules** are domain policies that govern when records can be archived or anonymized

## Compliance Patterns in DDD

| Compliance Requirement | DDD Pattern |
|----------------------|-------------|
| Audit trail | Domain events (immutable, timestamped, attributed) |
| Access control | Domain policies on aggregate operations |
| Consent management | Consent entity in Patient Records context |
| Data minimization | Minimal cross-context event payloads |
| Breach detection | Domain events for access anomaly detection |
| Record retention | Domain policies on aggregate lifecycle |
| Electronic signatures | Value objects with signer identity, intent, timestamp |

## Relationships to Other Topics

- [Domain Events](../domain-events/) -- Events provide the audit trail for compliance
- [Aggregates](../aggregates/) -- Compliance rules are enforced as aggregate invariants
- [Patient Records Context](../patient-records-context/) -- Patient data is the primary subject of privacy regulations
- [Pharmacy & Medication Context](../pharmacy-medication-context/) -- Controlled substance rules are enforced here
- [Insurance & Claims Context](../insurance-claims-context/) -- HIPAA Transaction Rule mandates EDI formats
- [Telemedicine Context](../telemedicine-context/) -- State licensure and telehealth consent rules apply
- [Medical Standards](../medical-standards/) -- HIPAA mandates specific standards for electronic transactions
- [Event-Driven Architecture](../event-driven-architecture/) -- Event stores provide durable audit logs

## References

- U.S. Department of Health and Human Services. "HIPAA Privacy Rule." 45 CFR Part 164. https://www.hhs.gov/hipaa/
- U.S. Department of Health and Human Services. "HIPAA Security Rule." 45 CFR Part 164 Subpart C.
- U.S. Congress. "HITECH Act." Title XIII of the American Recovery and Reinvestment Act, 2009.
- FDA. "21 CFR Part 11: Electronic Records; Electronic Signatures." https://www.ecfr.gov/
- European Parliament. "General Data Protection Regulation (GDPR)." Regulation (EU) 2016/679.
- DEA. "Title 21 Code of Federal Regulations." Parts 1301-1321. https://www.deadiversion.usdoj.gov/
- ONC. "Health IT Certification Program." https://www.healthit.gov/
- CCHP. "State Telehealth Laws and Reimbursement Policies." Center for Connected Health Policy.
- Tovino, Stacey. "The HIPAA Privacy Rule and the EU GDPR: Illustrative Comparisons." Seton Hall Law Review, 2017.
