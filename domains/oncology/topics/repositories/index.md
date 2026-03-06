# Repositories in Oncology

## Overview

Repositories provide an abstraction for storing and retrieving aggregate roots, shielding the domain model from persistence concerns. In oncology, repositories serve as the gateway to aggregate roots such as Cancer Diagnoses, Treatment Plans, Chemotherapy Orders, and Radiation Treatment Plans. The repository interface is defined in terms of the domain model's ubiquitous language, not in terms of database queries or storage mechanisms. This separation ensures that the oncology domain logic remains pure and independent of infrastructure.

## Repository Design Principles

Repositories in oncology follow several core principles. Each aggregate root has its own repository. The repository interface is defined in the domain layer using domain language. The implementation resides in the infrastructure layer. Repositories return fully reconstituted aggregates, not partial data projections. Query methods use domain-meaningful parameters, not raw database columns.

The repository interface expresses what the domain needs, not how persistence works. A ChemotherapyOrderRepository offers methods like findActiveOrdersForPatient and findOrdersByRegimen, not methods like findByColumn or executeQuery. This domain-aligned interface makes the repository a natural part of the ubiquitous language.

## Key Repositories by Bounded Context

### Diagnosis and Staging Context

**CancerDiagnosisRepository**: Provides access to Cancer Diagnosis aggregate roots. Key operations include retrieving a diagnosis by its identifier, finding all diagnoses for a specific patient, finding diagnoses by cancer type and stage for cohort analysis, and persisting new or updated diagnoses. The repository ensures that a retrieved diagnosis includes its complete TNM classification, molecular profile, and pathology references.

**PathologyCaseRepository**: Provides access to Pathology Case aggregate roots. Key operations include retrieving a case by accession number, finding cases pending pathologist review, and finding cases by specimen type or collection date. The repository returns complete pathology cases with all associated specimens and report sections.

### Treatment Planning Context

**TreatmentPlanRepository**: Provides access to Treatment Plan aggregate roots. Key operations include retrieving a plan by identifier, finding the active treatment plan for a patient, finding plans pending tumor board review, and finding plans by treatment intent or modality. The repository ensures that a retrieved treatment plan includes all modality selections, sequencing information, and tumor board decision references.

**ClinicalTrialEnrollmentRepository**: Provides access to Clinical Trial Enrollment aggregate roots. Key operations include finding enrollments by patient, finding active enrollments for a specific trial protocol, and finding enrollments by enrollment status. The repository supports the clinical trial matching and compliance tracking workflows.

### Chemotherapy Management Context

**ChemotherapyOrderRepository**: Provides access to Chemotherapy Order aggregate roots. Key operations include retrieving an order by order number, finding active orders for a patient, finding orders by regimen for protocol compliance reporting, and finding orders with pending dose modifications. The repository returns complete orders with all calculated doses, cycle records, and modification history.

**ToxicityAssessmentRepository**: Provides access to Toxicity Assessment aggregate roots. Key operations include finding assessments by patient and treatment cycle, finding assessments by CTCAE grade (supporting identification of severe toxicities), and finding assessments that triggered dose modifications.

### Radiation Therapy Context

**RadiationTreatmentPlanRepository**: Provides access to Radiation Treatment Plan aggregate roots. Key operations include retrieving a plan by identifier, finding the active plan for a patient, finding plans by treatment technique, and finding plans pending physics review. The repository ensures that retrieved plans include all target volume definitions, dose constraints, and fractionation schedules.

### Surgical Oncology Context

**SurgicalCaseRepository**: Provides access to Surgical Case aggregate roots. Key operations include retrieving a case by identifier, finding cases for a patient, finding cases by procedure type, and finding cases with positive margins requiring follow-up action. The repository returns complete surgical cases with operative details, margin assessments, and sentinel node results.

### Survivorship Care Context

**SurvivorshipCarePlanRepository**: Provides access to Survivorship Care Plan aggregate roots. Key operations include retrieving the current plan for a patient, finding plans with upcoming surveillance milestones, and finding plans by cancer type for guideline update propagation. The repository returns complete care plans with treatment summaries, follow-up schedules, and late effect monitoring plans.

## Repository Query Patterns

Oncology repositories support several query patterns aligned with clinical workflows. Temporal queries find aggregates based on clinical timeframes, such as chemotherapy orders started within a specific date range or follow-up visits due within the next month. Status-based queries find aggregates in specific lifecycle states, such as treatment plans pending approval or radiation courses in progress. Cohort queries find aggregates matching clinical criteria, such as all breast cancer diagnoses with HER2-positive status for quality reporting.

## Read Models and CQRS

Complex reporting and analytics needs in oncology are best served by separating read models from the write-oriented aggregate repositories. A treatment outcome dashboard requires data from multiple aggregates across multiple bounded contexts. Rather than forcing complex queries through aggregate repositories, a read model is built from domain events and serves reporting needs directly.

The CQRS (Command Query Responsibility Segregation) pattern separates the write side (commands that modify aggregates through repositories) from the read side (queries that serve reporting and analytics). In oncology, the write side ensures clinical data integrity through aggregate invariants, while the read side provides flexible views for tumor boards, quality reporting, and clinical research.

## Repository and Unit of Work

Repositories in oncology typically participate in a unit of work pattern that ensures all changes to an aggregate and its associated domain events are persisted atomically. When a chemotherapy order aggregate records a dose modification and publishes a DoseModificationApplied event, both the modified aggregate and the event must be persisted together. The repository implementation coordinates with the unit of work to guarantee this atomicity.

## Testing Repositories

Repository interfaces enable testing of domain logic without persistence infrastructure. In-memory repository implementations can be used in domain model tests, allowing aggregate behavior to be verified independently of database concerns. This separation is particularly valuable in oncology, where domain logic testing must be rigorous and fast.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6: The Life Cycle of a Domain Object.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12: Repositories.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11: CQRS.
