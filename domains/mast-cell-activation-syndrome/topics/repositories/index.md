# Repositories for Mast Cell Activation Syndrome

## Overview

Repositories provide abstractions for storing and retrieving aggregate roots. They decouple the domain model from persistence infrastructure, allowing the domain to remain focused on business logic without concern for how data is stored or retrieved. In the MCAS domain, repositories define the contracts for persisting clinical data while keeping the domain model free from database-specific concepts.

Each aggregate root has a corresponding repository. Repositories operate at the aggregate level, not at the entity or value object level. When an aggregate is retrieved, all its contained entities and value objects are loaded together, maintaining aggregate consistency.

## Diagnostic Assessment Repositories

### DiagnosticWorkupRepository

The DiagnosticWorkupRepository manages the persistence of Diagnostic Workup aggregates. It provides methods to store a new workup, retrieve a workup by its identifier, find workups by patient identifier, and query workups by diagnostic status. The repository ensures that when a workup is retrieved, all its Mediator Panels, Mediator Levels, and Criteria Evaluations are loaded as a complete aggregate.

Key operations include saving a workup with all its nested components atomically, finding active workups for a patient, retrieving completed workups with confirmed diagnoses, and listing workups pending interpretation. The repository does not expose methods for directly manipulating individual mediator levels or criteria evaluations; these are accessed through the aggregate root.

### BoneMarrowAssessmentRepository

The BoneMarrowAssessmentRepository handles persistence of Bone Marrow Assessment aggregates. It provides retrieval by assessment identifier, lookup by patient identifier, and filtering by assessment outcome. Because bone marrow assessments have a longer lifecycle than other diagnostic components, the repository supports querying by status to find assessments awaiting pathology results.

## Trigger Management Repositories

### TriggerProfileRepository

The TriggerProfileRepository manages Trigger Profile aggregates. Since each patient has exactly one trigger profile, the repository supports retrieval by patient identifier as a primary access pattern. The repository loads the complete profile with all Trigger Entry entities and their associated classifications and confidence levels.

The repository also supports query methods for finding profiles with specific trigger types, which enables population-level analysis of trigger prevalence. However, all modifications to trigger entries occur through the Trigger Profile aggregate root.

### AvoidancePlanRepository

The AvoidancePlanRepository handles Avoidance Plan aggregates. It provides retrieval by plan identifier and by patient identifier. The repository supports querying for plans that include specific environmental controls or dietary restrictions, enabling analysis of avoidance strategy patterns across the patient population.

## Medication Protocol Repositories

### MedicationRegimenRepository

The MedicationRegimenRepository manages Medication Regimen aggregates. It provides retrieval by regimen identifier and by patient identifier, returning the complete regimen with all medication entries and their titration plans. The repository supports querying for regimens containing specific medications, which supports population-level medication usage analysis.

The repository ensures that medication changes are persisted atomically with all related updates, including titration schedule modifications and compounding specification adjustments. Historical regimen states are preserved to support retrospective analysis of treatment evolution.

### CompoundingSpecificationRepository

The CompoundingSpecificationRepository handles Compounding Specification aggregates. It supports retrieval by specification identifier, by patient identifier, and by medication name. The repository also provides query methods for finding specifications with specific excipient exclusions, which supports pharmacovigilance analysis.

## Symptom Tracking Repositories

### SymptomLogRepository

The SymptomLogRepository manages Symptom Log aggregates. Given the potentially high volume of symptom data, the repository supports time-bounded retrieval, allowing callers to load symptom logs for specific date ranges rather than the entire history. The repository provides methods for retrieving the most recent log, finding logs containing entries above a severity threshold, and aggregating entry counts by organ system.

### FlareRecordRepository

The FlareRecordRepository handles Flare Record aggregates. It provides retrieval by flare identifier, listing by patient identifier with date range filtering, and querying by severity level. The repository supports analysis queries such as finding flares associated with specific triggers or occurring within defined time periods after medication changes.

## Specialist Coordination Repositories

### CareTeamRepository

The CareTeamRepository manages Care Team aggregates. It provides retrieval by team identifier and by patient identifier. The repository supports querying for teams that include providers of a specific specialty, enabling analysis of specialist involvement patterns.

### ReferralRepository

The ReferralRepository handles Referral aggregates. It provides retrieval by referral identifier, listing by patient identifier, and filtering by status and target specialty. The repository supports workflow queries such as finding pending referrals, overdue referrals, and recently completed referrals.

## Outcomes Measurement Repositories

### OutcomesAssessmentRepository

The OutcomesAssessmentRepository manages Outcomes Assessment aggregates. It provides retrieval by assessment identifier, listing by patient identifier with date ordering, and querying by score ranges. The repository supports longitudinal analysis by providing methods to retrieve assessment series for trend computation.

## Repository Design Principles

Repositories in the MCAS domain follow several design principles. They are defined as interfaces in the domain layer and implemented in the infrastructure layer. They operate exclusively on aggregate roots. They do not expose query methods that bypass aggregate boundaries. They support unit of work patterns for atomic persistence of aggregate changes.

Repositories do not contain business logic. Validation and invariant enforcement remain within the aggregates themselves. The repository's responsibility is limited to translating between the domain model and the persistence mechanism.

## Collection-Oriented vs. Persistence-Oriented

The MCAS domain uses a mix of collection-oriented and persistence-oriented repository styles. Collection-oriented repositories, which simulate an in-memory collection, are used for aggregates with moderate data volumes such as Care Teams and Medication Regimens. Persistence-oriented repositories, which require explicit save operations, are used for high-volume aggregates such as Symptom Logs.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on persistence and repositories.
