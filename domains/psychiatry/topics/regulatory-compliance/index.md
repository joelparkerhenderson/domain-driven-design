# Regulatory Compliance in the Psychiatry Domain

## Overview

Regulatory compliance encompasses the legal and governance requirements that shape the design of the psychiatry domain model. Psychiatric systems operate under some of the most stringent regulatory requirements in healthcare, including general health information privacy laws, specialized substance use confidentiality protections, involuntary commitment statutes, prescribing regulations for controlled substances, and quality reporting mandates. These requirements are not afterthoughts; they must be modeled as first-class business rules within the domain, influencing aggregate design, event publishing, data access patterns, and audit capabilities.

## HIPAA (Health Insurance Portability and Accountability Act)

HIPAA establishes the baseline privacy and security requirements for all protected health information (PHI) in the United States.

### Privacy Rule Impact on Domain Design

- **Minimum Necessary Standard**: Each bounded context must access only the minimum patient information necessary for its function. This aligns naturally with DDD's principle of context-specific patient representations. The Medication Management Context does not need a patient's therapy session notes; it only needs the patient's medication-relevant attributes.
- **Patient Access Rights**: Patients have the right to access their psychiatric records. The domain model must support generating patient-facing record exports that compile data across bounded contexts into a comprehensible format.
- **Accounting of Disclosures**: Every disclosure of PHI to external parties must be logged. Domain events that cross organizational boundaries (e.g., prescription transmissions to pharmacies, state reporting submissions) must generate disclosure accounting entries.

### Security Rule Impact on Domain Design

- **Access Controls**: Repository implementations must enforce role-based access. A crisis responder may access shared kernel data; a billing analyst may not.
- **Audit Logging**: All read and write operations on PHI must be auditable. Domain events inherently provide a write audit trail; read access must be separately logged.
- **Data Integrity**: Aggregate invariants must prevent unauthorized modification of clinical records. Finalized psychiatric evaluations, signed prescriptions, and completed crisis event records must be immutable.

## 42 CFR Part 2 (Confidentiality of Substance Use Disorder Patient Records)

This federal regulation provides heightened confidentiality protections for substance use disorder (SUD) treatment records, beyond what HIPAA requires.

### Domain Model Impact

- **Segmentation**: SUD-related diagnoses, treatment records, and clinical notes must be segmentable from general psychiatric records. The domain model must support granular consent-based data sharing where SUD records require separate, specific patient consent before disclosure.
- **Consent Management**: A patient may consent to share their depression diagnosis with a primary care physician but not their opioid use disorder diagnosis. The domain model must support diagnosis-level consent granularity, which is modeled as a cross-cutting concern with business rules enforced at the repository and event publishing levels.
- **Re-disclosure Prohibition**: When SUD records are disclosed with consent, the receiving party is prohibited from re-disclosing them. Published events containing SUD-related data must carry re-disclosure prohibition markers.
- **Recent Amendments (2024)**: The CARES Act amendments aligned certain 42 CFR Part 2 provisions with HIPAA for treatment, payment, and healthcare operations, but significant restrictions remain. The domain model must reflect the current regulatory state.

## State Mental Health Laws

Involuntary commitment and treatment laws vary significantly by state, directly affecting the Crisis Management Context.

### Involuntary Hold Statutes

- **Variation**: California uses 5150 (72-hour) and 5250 (14-day) holds. Florida uses the Baker Act. New York uses Section 9.39 of the Mental Hygiene Law. Each state has different criteria, durations, documentation requirements, and judicial review processes.
- **Domain Model Impact**: The Involuntary Hold Type value object must be jurisdiction-configurable. Hold statutes, maximum durations, required documentation, patient rights notification timelines, and hearing schedules are modeled as configurable business rules rather than hardcoded logic.
- **Patient Rights**: Each jurisdiction mandates specific patient rights during involuntary holds (right to counsel, right to hearing, right to contact family). The domain model must track rights notification and exercise.

### Mandatory Reporting

- **Duty to Warn/Protect**: Following the Tarasoff ruling and its state-level implementations, clinicians have obligations to warn identifiable potential victims of patient threats. This triggers specific documentation and notification workflows in the Crisis Management Context.
- **Child and Elder Abuse Reporting**: Mandatory reporting obligations create domain events that must flow to external reporting systems through anti-corruption layers.

## Controlled Substance Regulations

### DEA (Drug Enforcement Administration)

- **Schedule Classification**: Controlled substances (Schedule II-V) have prescribing restrictions that the Medication Management Context must enforce. Schedule II substances (e.g., amphetamine stimulants) require more restrictive prescribing rules than Schedule IV substances (e.g., benzodiazepines).
- **EPCS (Electronic Prescribing for Controlled Substances)**: DEA regulations require specific identity proofing and two-factor authentication for electronic controlled substance prescribing. The Prescription aggregate must enforce EPCS compliance for controlled substances.
- **PDMP (Prescription Drug Monitoring Program)**: Most states require prescribers to check the PDMP before prescribing controlled substances. This external system check is handled through an anti-corruption layer, with the check result recorded as part of the Prescription aggregate.

## Quality Reporting Requirements

### CMS Quality Measures

The Centers for Medicare and Medicaid Services mandates quality reporting for participating providers. The Outcomes Measurement Context must support:

- **MIPS (Merit-based Incentive Payment System)**: Quality measures, improvement activities, and promoting interoperability measures.
- **Behavioral Health Quality Measures**: Specific measures for depression follow-up, antidepressant management, metabolic monitoring, and follow-up after psychiatric hospitalization.

### Joint Commission Standards

For accredited facilities, Joint Commission standards require:
- Suicide risk assessment documentation and safety planning.
- Restraint and seclusion monitoring and reporting.
- Medication reconciliation at transitions of care.
- Patient safety event reporting.

## Regulatory Compliance as Business Rules

In the DDD approach, regulatory requirements are modeled as business rules within the domain, not as external compliance checks applied after the fact. This means:

- Aggregate invariants encode regulatory constraints (e.g., a prescription cannot be finalized without PDMP verification for controlled substances).
- Domain events carry compliance metadata (e.g., 42 CFR Part 2 re-disclosure markers).
- Repository access controls enforce minimum necessary and consent-based data segmentation.
- Anti-corruption layers ensure that external regulatory reporting receives properly formatted data.

This approach ensures that the system is compliant by design rather than compliant by audit.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- U.S. Department of Health and Human Services. *HIPAA Privacy Rule*. 45 CFR Parts 160, 164.
- Substance Abuse and Mental Health Services Administration. *42 CFR Part 2*. SAMHSA, 2024.
- Tarasoff v. Regents of the University of California, 17 Cal. 3d 425 (1976).
- Drug Enforcement Administration. *Electronic Prescriptions for Controlled Substances*. 21 CFR Parts 1304, 1306, 1311.
