# Regulatory Compliance in Ophthalmology

## Overview

The ophthalmology domain operates under a complex regulatory landscape that shapes
domain model design, data handling practices, audit requirements, and integration
patterns. Regulatory compliance is a cross-cutting concern that affects all bounded
contexts. In Domain-Driven Design, regulatory requirements are treated as business
rules and invariants within the domain model rather than as afterthoughts applied
at the infrastructure level.

## HIPAA (Health Insurance Portability and Accountability Act)

### Privacy Rule

The HIPAA Privacy Rule governs the use and disclosure of protected health information
(PHI). In the ophthalmology domain, PHI includes patient demographic data, clinical
examination findings, imaging data, surgical records, and treatment histories.

Impact on domain model:
- All domain aggregates containing PHI must enforce access controls through the
  domain layer. The domain model defines who may access clinical data based on
  clinical role and treatment relationship.
- Patient consent for data sharing must be modeled as a domain concept, with consent
  status checked before data is shared across contexts or with external systems.
- Minimum necessary standard: domain queries and domain events must carry only the
  minimum PHI necessary for the consuming context to perform its function.

### Security Rule

The HIPAA Security Rule requires administrative, physical, and technical safeguards
for electronic PHI (ePHI). While many security controls are implemented at the
infrastructure level, the domain model contributes to compliance through:

- Audit logging of all clinical data access and modifications. Domain events serve
  as an immutable audit trail of clinical activity.
- User authentication and authorization modeled as domain concerns, ensuring that
  clinical data access is governed by domain rules.
- Data integrity controls within aggregates, preventing unauthorized modification
  of clinical records.

### Breach Notification Rule

The domain model supports breach notification by maintaining audit logs that can
identify what data was accessed, by whom, and when. Domain events provide the
traceability needed to assess the scope of a potential breach.

## FDA Regulations

### Medical Device Regulations (21 CFR Part 820)

Ophthalmology systems that qualify as medical devices (e.g., systems that provide
clinical decision support, automated image analysis, or IOL calculation) must comply
with FDA Quality System Regulations.

Impact on domain model:
- IOLCalculationService and other clinical calculation services must implement
  validated algorithms with documented verification and validation evidence.
- Software changes affecting clinical decision-making require documented change
  control processes.
- Clinical decision support services must clearly distinguish between automated
  recommendations and clinician judgment.

### 21 CFR Part 11 (Electronic Records, Electronic Signatures)

Part 11 governs electronic records and signatures used in FDA-regulated activities.

Impact on domain model:
- Surgical consent (SurgicalConsent entity) must support electronic signature
  requirements including signer identification, signature meaning, and timestamp.
- Clinical records must maintain audit trails showing creation, modification, and
  deletion of electronic records.
- Domain aggregates must support version control and amendment tracking to comply
  with record integrity requirements.

## Clinical Coding Compliance

### ICD-10 Coding Accuracy

Regulatory requirements mandate accurate diagnosis coding. The domain model supports
coding compliance by:

- Structuring clinical findings in a way that maps cleanly to ICD-10 codes.
- Providing coded diagnosis suggestions based on documented findings through domain
  services.
- Enforcing that surgical procedures have documented medical necessity linked to
  supporting diagnoses.

### CPT Coding Compliance

Procedure coding must accurately reflect the services performed. The domain model
supports CPT compliance by:

- Documenting examination elements that support the billed level of service.
- Tracking surgical procedure details that correspond to specific CPT codes.
- Recording bilateral versus unilateral procedures for correct modifier assignment.

## Clinical Trial Regulations (21 CFR Parts 50, 56, 312)

Ophthalmology practices participating in clinical trials must comply with FDA
regulations governing informed consent, institutional review boards, and
investigational drug/device protocols.

Impact on domain model:
- The Retinal Care Context may need to distinguish between standard-of-care
  treatment and clinical trial protocols for anti-VEGF therapies.
- The Surgical Management Context may track investigational devices or techniques
  under clinical trial protocols.
- Domain events must support data export in formats required by clinical trial
  sponsors and regulatory authorities.

## State Regulations

### Scope of Practice

State regulations define the scope of practice for ophthalmologists, optometrists,
and ophthalmic technicians. The domain model enforces scope-of-practice rules by:

- Restricting certain clinical actions (e.g., surgical consent, medication prescribing)
  to appropriately credentialed clinicians.
- Documenting the supervising clinician for services performed by trainees or
  supervised practitioners.

### Licensure Requirements

The domain model tracks clinician credentials and licensure status as domain concepts,
ensuring that clinical actions are performed by appropriately licensed providers.

## Compliance Monitoring

- Domain events provide a continuous audit trail for compliance monitoring.
- Aggregate invariants enforce regulatory business rules at the domain layer.
- Anti-corruption layers ensure that data exchanged with external systems meets
  regulatory format and content requirements.
- Compliance rules are modeled as explicit domain rules rather than implicit
  infrastructure controls, making them visible, testable, and auditable.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- U.S. Department of Health and Human Services. *HIPAA Privacy Rule*. 45 CFR Part 164.
- U.S. Food and Drug Administration. *Quality System Regulation*. 21 CFR Part 820.
- U.S. Food and Drug Administration. *Electronic Records; Electronic Signatures*. 21 CFR Part 11.
