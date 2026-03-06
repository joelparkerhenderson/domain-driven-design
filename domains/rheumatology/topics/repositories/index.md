# Repositories in the Rheumatology Domain

## Overview

Repositories provide abstractions for storing and retrieving aggregate roots, shielding the domain model from persistence infrastructure details. In the rheumatology domain, repositories serve as the interface between clinical domain logic and the underlying data store. They allow the domain model to focus purely on clinical rules and workflows without concerning itself with database schemas, query optimization, or storage technology. Each aggregate root in the domain has a corresponding repository interface defined in the domain layer.

## Repository Design Principles

Repositories in the rheumatology domain follow several foundational principles from DDD. They operate exclusively on aggregate roots, not on individual entities or value objects within an aggregate. They provide collection-like semantics (add, remove, find) rather than database-oriented operations (insert, update, delete). They are defined as interfaces in the domain layer with implementations in the infrastructure layer. They return fully reconstituted aggregates with all internal entities and value objects properly assembled.

The repository interface uses domain language, not technical language. A method is named "findActiveAssessmentsForPatient" rather than "selectByPatientIdAndStatusActive." Query parameters use domain value objects rather than primitive types.

## Clinical Assessment Context Repositories

### AssessmentRepository

The AssessmentRepository manages PatientAssessment aggregate roots. It provides operations to store a new assessment, retrieve an assessment by its unique identifier, find all assessments for a specific patient, find assessments within a date range, find the most recent completed assessment for a patient, and find assessments with specific disease activity score ranges.

The repository must reconstitute the full PatientAssessment aggregate including embedded JointExamination entities, SerologyResult value objects, and calculated DiseaseActivityScore value objects. When retrieving assessments for longitudinal comparison, the repository returns them in chronological order.

Specialized queries support clinical workflows: finding all patients due for reassessment based on their last assessment date and their treat-to-target interval, and finding assessments where disease activity exceeds a specified threshold.

## Autoimmune Disease Context Repositories

### DiseaseRepository

The DiseaseRepository manages DiseaseManagementPlan aggregate roots. It provides operations to store a management plan, retrieve a plan by identifier, find all active plans for a patient, find plans by disease type, and find plans where the current disease activity state indicates a flare.

The repository reconstitutes plans with their embedded AutoimmuneDisease entities, TreatmentGoal value objects, MedicationRegimen entities, and FlareEpisode entities. It supports queries for treatment escalation reviews: finding plans where the current therapy has been active for a specified duration without achieving the treatment target.

A critical query finds all patients on a specific medication who may be affected by a drug safety alert, supporting pharmacovigilance workflows.

## Joint Disease Context Repositories

### JointDiseaseRepository

The JointDiseaseRepository manages JointDiseasePlan aggregate roots. It provides operations to store a plan, retrieve by identifier, find plans by disease type (osteoarthritis, gout, pseudogout), and find patients due for serum urate monitoring.

For gout management, the repository supports queries to find patients whose serum urate remains above target despite therapy, identifying candidates for dose adjustment. For joint injection tracking, it finds joints that have received the maximum recommended number of injections within a time period.

## Biologic Therapy Context Repositories

### TherapyRepository

The TherapyRepository manages TherapyRegimen aggregate roots. It provides operations to store a therapy regimen, retrieve by identifier, find active therapy regimens for a patient, find regimens by drug or drug class, and find regimens requiring safety monitoring.

The repository supports infusion center operations: finding all patients with infusion sessions scheduled within a date range, finding patients due for pre-treatment screening renewal (such as annual tuberculosis re-screening), and finding patients on a specific reference biologic who are candidates for biosimilar switching.

Drug-specific queries support pharmacovigilance: finding all patients currently receiving a specific drug, finding patients who have experienced adverse events on a drug class, and generating therapy duration reports.

## Pain Management Context Repositories

### PainManagementRepository

The PainManagementRepository manages PainManagementPlan aggregate roots. It provides operations to store a plan, retrieve by identifier, find active plans for a patient, and find plans with pending therapy referrals.

The repository supports referral coordination: finding patients with therapy referrals that have not been acknowledged by the receiving provider, and finding patients whose therapy progress reports are overdue. It also supports outcome analysis: finding plans where pain levels have not improved after a specified treatment duration.

## Outcomes Tracking Context Repositories

### OutcomeRepository

The OutcomeRepository manages OutcomeReport aggregate roots. It provides operations to store an outcome report, retrieve by identifier, find all outcome records for a patient, find records by measurement instrument, and retrieve longitudinal trajectories for a specific patient and instrument combination.

The repository supports treat-to-target monitoring: finding patients who have not achieved their disease activity target within a specified time frame, and finding patients whose HAQ-DI scores have worsened over consecutive assessments. It supports quality reporting: aggregating outcome measures across patient populations for regulatory quality measure submission.

Longitudinal queries are particularly important in this repository. Retrieving a DAS28 trajectory requires assembling a chronologically ordered series of DAS28 scores with associated treatment landmarks (therapy starts, switches, escalations) to visualize the relationship between treatment decisions and disease activity.

## Repository Implementation Considerations

Repository implementations must handle the clinical data integrity requirements of rheumatology. Concurrent access by multiple clinicians to the same patient's data requires optimistic concurrency control. Audit trails must capture who accessed and modified clinical data and when. Data retention must comply with medical record keeping regulations.

The repository abstraction ensures that the domain model remains independent of these implementation concerns. The domain layer defines what data access it needs; the infrastructure layer decides how to fulfill those needs.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5.
- Fowler, Martin. Patterns of Enterprise Application Architecture. Addison-Wesley, 2002.
