# Repositories in the Dermatology Domain

Repositories are abstractions that provide the illusion of an in-memory collection of aggregate roots, mediating between the domain model and data storage. In the dermatology domain, repositories define the interface through which aggregates are stored, retrieved, and queried without exposing persistence infrastructure details to the domain layer. As Evans (2003) establishes, repositories encapsulate the logic needed to access data sources, keeping the domain model pure and independent of storage technology.

## Repository Design Principles

Repositories in the dermatology domain follow several key principles. Each repository manages a single aggregate root type. Repository interfaces are defined in the domain layer using domain language, while implementations reside in the infrastructure layer. Repositories return fully reconstituted aggregates with all invariants intact. Query methods use domain-meaningful criteria rather than database-specific constructs. Following Vernon's (2013) guidance, repositories avoid exposing generic query capabilities that would bypass aggregate boundaries.

## Clinical Assessment Context Repositories

### SkinExaminationRepository

The SkinExaminationRepository provides access to Skin Examination aggregates. It supports retrieval by examination identifier, retrieval of all examinations for a given patient identifier, retrieval of examinations within a date range for a patient, and retrieval of examinations containing flagged suspicious lesion findings. The repository reconstitutes the complete Skin Examination aggregate including all Lesion Finding entities and their associated Dermoscopy Evaluation value objects. When saving, the repository ensures that all lesion findings have valid anatomical locations and that the examination state is consistent.

### PatientSkinProfileRepository

The PatientSkinProfileRepository provides access to Patient Skin Profile aggregates. It supports retrieval by patient identifier and querying by Fitzpatrick skin type or documented skin conditions. The repository ensures that the profile's complete examination history references are intact when reconstituting the aggregate.

## Lesion Management Context Repositories

### LesionRecordRepository

The LesionRecordRepository is the most heavily used repository in the Lesion Management Context. It supports retrieval by lesion identifier, retrieval of all active lesion records for a patient, retrieval of lesions by diagnosis category, retrieval of lesions pending biopsy results, retrieval of lesions overdue for follow-up based on their scheduled follow-up intervals, and retrieval of lesions with confirmed malignant diagnoses. The repository reconstitutes the full Lesion Record aggregate including biopsy decisions, excision plans, and the complete assessment history. Query methods for follow-up scheduling use domain concepts (follow-up interval, last assessment date, risk category) rather than raw date arithmetic.

### FollowUpScheduleRepository

The FollowUpScheduleRepository provides access to Follow-Up Schedule aggregates. It supports retrieval of schedules for a specific patient, retrieval of upcoming follow-up appointments within a date range, and identification of overdue follow-up visits. The repository works with domain-meaningful time concepts such as follow-up intervals and risk-based scheduling rules.

## Procedure Management Context Repositories

### ProcedureSessionRepository

The ProcedureSessionRepository provides access to Procedure Session aggregates across all procedure types. It supports retrieval by session identifier, retrieval of sessions for a specific patient, retrieval by procedure type (Mohs, cryotherapy, laser, electrosurgery), retrieval of sessions for a specific lesion reference, and retrieval of sessions within a date range. When reconstituting a Procedure Session, the repository loads all procedure-type-specific child entities (Mohs stages, cryotherapy applications, laser passes) based on the procedure type.

### PhototherapyPlanRepository

The PhototherapyPlanRepository provides access to Phototherapy Plan aggregates. It supports retrieval by plan identifier, retrieval of active plans for a patient, and retrieval of plans with sessions due within a specified period. The repository reconstitutes the plan with its complete session history, enabling the aggregate to enforce cumulative dose limits and escalation rules.

## Cosmetic Services Context Repositories

### CosmeticTreatmentPlanRepository

The CosmeticTreatmentPlanRepository provides access to Cosmetic Treatment Plan aggregates. It supports retrieval by plan identifier, retrieval of active plans for a patient, retrieval by treatment type (neuromodulator, filler, chemical peel, microneedling), and retrieval of plans with treatments due for repeat sessions based on product duration expectations. The repository reconstitutes the full plan including injectable treatment records with product lot references.

### CosmeticConsentRepository

The CosmeticConsentRepository provides access to consent records. It supports retrieval by consent identifier, retrieval of active consents for a patient, and identification of expiring or expired consents. The repository ensures that consent lifecycle state is accurately represented.

## Pathology Integration Context Repositories

### SpecimenCaseRepository

The SpecimenCaseRepository provides access to Specimen Case aggregates. It supports retrieval by specimen accession number, retrieval of pending cases awaiting results, retrieval of cases for a specific patient, and retrieval of cases by diagnostic category. The repository reconstitutes the complete chain of custody and any associated histopathology reports. This repository may coordinate with an anti-corruption layer when the underlying data is stored in an external laboratory information system.

## Treatment Outcomes Context Repositories

### TreatmentOutcomeRecordRepository

The TreatmentOutcomeRecordRepository provides access to Treatment Outcome Record aggregates. It supports retrieval by outcome identifier, retrieval of outcomes for a specific patient, retrieval of outcomes by treatment modality, retrieval of outcomes for a specific lesion or condition, and retrieval of outcomes with specific response classifications. The repository reconstitutes the complete longitudinal record including all severity assessments and wound healing observations.

## Repository vs. Query Service

Following the CQRS principle referenced in the domain architecture, repositories serve the command (write) side of the system, providing access to aggregates for state changes. Read-optimized queries that span multiple aggregates or require complex filtering (such as dashboards showing all patients with overdue follow-ups across multiple lesions) are handled by separate query services that operate on read-optimized projections rather than through aggregate repositories.

## Collection-Oriented vs. Persistence-Oriented

Khononov (2021) distinguishes between collection-oriented repositories (which mimic in-memory collections) and persistence-oriented repositories (which make persistence operations explicit). The dermatology domain favors collection-oriented repositories for most contexts, providing add, remove, and find operations that hide the underlying persistence mechanism. The Pathology Integration Context may use a persistence-oriented approach when interacting with external laboratory systems that require explicit save and sync operations.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repository design and implementation.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on repository patterns.
