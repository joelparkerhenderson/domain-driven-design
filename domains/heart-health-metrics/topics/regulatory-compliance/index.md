# Regulatory Compliance

Regulatory compliance in the heart health metrics domain addresses the legal, privacy, and medical device regulatory requirements that shape the design of cardiac monitoring systems. Compliance is not an afterthought but a cross-cutting concern that influences domain model design, data handling policies, audit capabilities, and system architecture decisions across all bounded contexts.

## Overview

Cardiac health monitoring systems operate under stringent regulatory frameworks because they collect, process, and transmit protected health information (PHI) and may qualify as medical device software. The heart health metrics domain must comply with HIPAA for health data privacy and security, FDA regulations for software as a medical device (SaMD), GDPR for patients in European jurisdictions, and various national and regional health data protection laws.

Regulatory requirements influence domain modeling in concrete ways: audit trail requirements affect how domain events are stored, data retention policies constrain repository design, access control requirements shape bounded context interfaces, and medical device classification determines the rigor of validation and verification processes.

## Key Concepts

**HIPAA (Health Insurance Portability and Accountability Act).** The foundational U.S. regulation for health data privacy and security. HIPAA requires that all PHI (patient identifiers, cardiac measurements, clinical findings) be protected through administrative, physical, and technical safeguards. The heart health metrics domain must ensure encryption at rest and in transit, access logging, minimum necessary access, and breach notification capabilities.

**FDA Software as a Medical Device (SaMD).** The FDA regulates software that performs a medical function, such as detecting arrhythmias or computing cardiovascular risk scores. The Cardiac Analysis Context and Risk Assessment Context likely qualify as SaMD and must follow FDA design controls, including requirements documentation, design verification and validation, risk analysis, and post-market surveillance. The Data Collection Context may also qualify if it processes signals from regulated medical devices.

**GDPR (General Data Protection Regulation).** For patients in European jurisdictions, GDPR imposes requirements including explicit consent for health data processing, data minimization, the right to access and portability, the right to erasure (subject to medical record retention laws), and data protection impact assessments. These requirements influence how patient consent is modeled and how data deletion is implemented.

**Audit Trail Requirements.** Regulatory frameworks universally require comprehensive audit trails for cardiac health data. Every access, modification, and transmission of PHI must be logged. Domain events serve a dual purpose: they drive business logic and provide an immutable audit trail. The event sourcing pattern naturally supports regulatory audit requirements.

**Data Retention and Destruction.** Medical record retention laws vary by jurisdiction but typically require cardiac health records to be retained for 7-10 years (longer for minors). The repository layer must enforce retention policies and support secure data destruction when retention periods expire. Retention policies must balance regulatory minimums with clinical research value.

**Informed Consent.** The domain model must capture patient consent for data collection, analysis, and sharing. Consent is not a simple boolean; it specifies the scope of data collection (which metrics), the purpose (clinical monitoring, research, quality improvement), the sharing permissions (which clinicians, which systems), and the duration. Consent may be withdrawn, requiring cascading data handling adjustments.

**International Medical Device Regulation (MDR/IVDR).** For European markets, the Medical Device Regulation (EU 2017/745) establishes requirements for medical device software classification, conformity assessment, post-market surveillance, and unique device identification that complement FDA requirements.

## Domain Examples

A cardiac monitoring application that detects atrial fibrillation undergoes FDA 510(k) clearance. The Cardiac Analysis Context's rhythm classification algorithm must be validated against a predicate device, with documented sensitivity and specificity for AFib detection. The design control documentation traces requirements through design, verification, and validation. Post-market surveillance monitors the algorithm's real-world performance.

A telehealth company offering remote blood pressure monitoring to patients in both the United States and Germany implements HIPAA and GDPR compliance simultaneously. The domain model includes a Consent aggregate that captures jurisdiction-specific consent requirements, a data handling policy engine that applies the stricter of applicable regulations, and an audit service that logs all PHI access with sufficient detail for both HIPAA and GDPR compliance reviews.

## Relationships to Other Topics

- [Repositories](../repositories/index.md) - Repositories must support audit and retention requirements.
- [Domain Events](../domain-events/index.md) - Events provide audit trail and compliance logging.
- [Clinical Integration Context](../clinical-integration-context/index.md) - Interoperability mandates drive integration compliance.
- [Heart Health Metrics Standards](../heart-health-metrics-standards/index.md) - Standards support regulatory compliance evidence.
- [Bounded Contexts](../bounded-contexts/index.md) - Compliance requirements influence context boundaries.
- [Event-Driven Architecture](../event-driven-architecture/index.md) - Event sourcing supports audit requirements.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021.
- U.S. Department of Health and Human Services. "HIPAA Privacy Rule." 45 CFR Parts 160, 164.
- FDA. "Software as a Medical Device (SaMD): Clinical Evaluation." U.S. Food and Drug Administration, 2017.
- FDA. "Design Controls Guidance for Medical Device Manufacturers." 21 CFR Part 820.
- European Parliament. "Regulation (EU) 2016/679 (GDPR)." Official Journal of the European Union, 2016.
- European Parliament. "Regulation (EU) 2017/745 on Medical Devices (MDR)." Official Journal of the European Union, 2017.
- IMDRF. "Software as a Medical Device: Possible Framework." International Medical Device Regulators Forum, 2014.
