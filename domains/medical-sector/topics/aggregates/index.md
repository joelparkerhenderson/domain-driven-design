# Aggregates in Medical Practice

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Every aggregate has a root entity through which all external access is managed. In medical practice, aggregates model the natural consistency boundaries of clinical data -- a patient record with its problem list, a prescription with its medication details, or a claim with its service lines.

## Why Aggregates Matter in Medicine

Medical data has strict consistency requirements driven by patient safety and regulatory compliance:

- A prescription must always have valid dosage, route, and frequency before it can be issued
- A lab result must be associated with the correct patient and ordering provider
- A claim must contain consistent diagnosis and procedure codes before submission
- A patient's allergy list must never allow duplicate entries for the same substance

Aggregates enforce these invariants by defining transactional boundaries that guarantee data consistency within the boundary and eventual consistency across boundaries.

## Key Aggregates in the Medical Domain

### PatientRecord Aggregate (Patient Records Context)

- **Aggregate Root**: PatientRecord (identified by MRN)
- **Internal Entities**: None (all components are value objects)
- **Value Objects**: Demographics, ContactInfo, EmergencyContact, Allergy, InsurancePolicy
- **Collections**: ProblemList (list of ActiveCondition and ResolvedCondition), MedicalHistory entries
- **Invariants**: MRN is unique and immutable; no duplicate allergies; at most one primary insurance policy; date of birth cannot be in the future

### Encounter Aggregate (Clinical Workflow Context)

- **Aggregate Root**: Encounter (identified by EncounterID)
- **Internal Entities**: ClinicalNote
- **Value Objects**: ChiefComplaint, Assessment, Diagnosis (ICD-10 code + description), VitalSigns
- **Collections**: Orders (lab, imaging, medication, referral), ClinicalNotes
- **Invariants**: An encounter must have an associated patient and provider; an encounter cannot be closed without at least one assessment

### Prescription Aggregate (Pharmacy & Medication Context)

- **Aggregate Root**: Prescription (identified by PrescriptionID)
- **Value Objects**: Medication (RxNorm code, NDC), Dosage, Quantity, RefillAuthorization, PrescriberInfo
- **Invariants**: Controlled substances require a valid DEA number; dosage must be within safe ranges; refills cannot exceed maximum allowed

### LabOrder Aggregate (Diagnostics & Imaging Context)

- **Aggregate Root**: LabOrder (identified by OrderID)
- **Value Objects**: TestPanel, SpecimenRequirement, ClinicalIndication, Priority
- **Invariants**: A lab order must have at least one test; priority must be a valid level (routine, urgent, stat)

### Claim Aggregate (Insurance & Claims Context)

- **Aggregate Root**: Claim (identified by ClaimID)
- **Internal Entities**: ServiceLine
- **Value Objects**: DiagnosisCode (ICD-10), ProcedureCode (CPT), PlaceOfService, BillingProvider
- **Invariants**: A claim must have at least one service line; each service line must have a valid CPT code and at least one linked diagnosis code

### VirtualVisit Aggregate (Telemedicine Context)

- **Aggregate Root**: VirtualVisit (identified by VisitID)
- **Value Objects**: SessionDetails, ConnectionInfo, TelehealthConsent, VisitDuration
- **Invariants**: A virtual visit requires active telehealth consent; session cannot start without verified patient identity

## Aggregate Design Rules

- **Keep aggregates small**: Prefer smaller aggregates with fewer internal entities to minimize contention and locking
- **Reference by identity**: Aggregates reference other aggregates by ID, not by direct object reference
- **One transaction per aggregate**: Each business operation modifies exactly one aggregate; cross-aggregate consistency uses domain events
- **Protect invariants**: The aggregate root is responsible for enforcing all business rules within its boundary

## Relationships to Other Topics

- [Entities](../entities/) -- Aggregate roots are entities; aggregates may contain internal entities
- [Value Objects](../value-objects/) -- Aggregates contain value objects that carry domain meaning
- [Domain Events](../domain-events/) -- Aggregates publish domain events when significant state changes occur
- [Repositories](../repositories/) -- Repositories persist and retrieve aggregate roots
- [Domain Services](../domain-services/) -- Domain services coordinate operations across multiple aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10. Addison-Wesley, 2013.
- Vernon, Vaughn. "Effective Aggregate Design." Three-part series, 2011. https://www.dddcommunity.org/
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 19. Wrox, 2015.
