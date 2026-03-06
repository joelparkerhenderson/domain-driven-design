# Repositories in the Urology Domain

## Overview

A repository is an abstraction that mediates between the domain model and the data mapping layer, providing a collection-like interface for accessing aggregate roots. In the urology domain, repositories encapsulate the persistence and retrieval of clinical aggregates, shielding the domain model from infrastructure concerns such as database technology, query optimization, and data serialization. The domain model interacts with repositories as if they were in-memory collections of domain objects.

## Repository Design Principles

Repositories in the urology domain follow several guiding principles. Each repository manages a single aggregate root type. Repositories expose domain-meaningful query methods rather than generic data access operations. The repository interface is defined in the domain layer, while the implementation resides in the infrastructure layer. This separation ensures that the domain model remains independent of persistence technology.

## Clinical Assessment Repositories

### DiagnosticWorkupRepository

This repository manages the persistence and retrieval of DiagnosticWorkup aggregates. It provides methods to find workups by patient identifier, by date range, by diagnostic conclusion, and by presenting symptom. The repository supports retrieval of the most recent workup for a patient, which is frequently needed by downstream treatment contexts. It also provides a method to find incomplete workups awaiting diagnostic test results.

### UrodynamicStudyRepository

This repository manages UrodynamicStudy aggregates. It provides retrieval by patient identifier, by study date, and by study indication (pre-operative evaluation, post-treatment assessment, initial evaluation). The repository supports comparison queries that retrieve all studies for a patient in chronological order, enabling trend analysis of bladder function parameters over time.

## Surgical Management Repositories

### SurgicalCaseRepository

This repository manages SurgicalCase aggregates. It provides retrieval by case number, by patient identifier, by surgeon, by procedure type, by date range, and by complication status. The repository supports aggregate queries for quality metrics: complication rates by procedure type, average operative times, and conversion rates from minimally invasive to open approach. These aggregate queries return read-only projections, not full aggregate roots.

### RoboticProcedureRepository

This repository manages RoboticProcedure aggregates as a specialized extension of the SurgicalCaseRepository. It adds retrieval methods specific to robotic surgery: by robotic platform type, by console time range, and by port configuration. It supports institutional reporting requirements for robotic surgery program metrics.

## Oncologic Urology Repositories

### CancerDiagnosisRepository

This repository manages CancerDiagnosis aggregates. It provides retrieval by patient identifier, by cancer type (prostate, bladder, kidney, testicular), by stage, by risk group, and by treatment status. The repository supports population-level queries for research and quality reporting, such as retrieving all prostate cancer diagnoses with ISUP Grade Group 2 or higher for treatment outcome analysis.

### SurveillancePlanRepository

This repository manages SurveillancePlan aggregates. It provides retrieval by patient identifier and by plan status (active, completed, discontinued). A critical method retrieves all plans with overdue surveillance visits, enabling the clinical team to track protocol compliance and contact patients who have missed scheduled monitoring appointments.

## Stone Disease Repositories

### StoneEpisodeRepository

This repository manages StoneEpisode aggregates. It provides retrieval by patient identifier, by stone location, by stone composition, by treatment modality, and by outcome (stone-free, residual fragments). The repository supports recurrence analysis by retrieving all episodes for a patient in chronological order, enabling calculation of recurrence intervals and identification of recurrence patterns.

### MetabolicProfileRepository

This repository manages MetabolicProfile aggregates. It provides retrieval by patient identifier and by metabolic abnormality type. The repository supports retrieval of serial metabolic profiles for a patient, enabling assessment of whether dietary and pharmacologic interventions are effectively modifying urinary risk factors over time.

## Incontinence Management Repositories

### ContinenceAssessmentRepository

This repository manages ContinenceAssessment aggregates. It provides retrieval by patient identifier, by incontinence type, by severity, and by treatment phase (initial evaluation, post-conservative therapy, post-surgical). The repository supports outcome analysis by retrieving pre-treatment and post-treatment assessments for the same patient to calculate improvement metrics.

## Male Reproductive Health Repositories

### FertilityEvaluationRepository

This repository manages FertilityEvaluation aggregates. It provides retrieval by patient identifier, by evaluation status (in-progress, complete), and by diagnostic conclusion (normal, oligozoospermia, azoospermia, hormonal abnormality). The repository supports retrieval of serial semen analyses for a patient, enabling trend analysis and treatment response assessment.

## Query Strategies

Repositories in the urology domain employ two query strategies. Simple lookups (by identifier, by patient) load the full aggregate root for command operations. Complex queries (by clinical criteria, for reporting) use read-optimized projections that return only the data needed for the specific use case. This follows the CQRS principle of separating read and write models, where the repository serves the write side and separate read models serve complex queries.

## Repository and Domain Events

When a repository persists changes to an aggregate, it also publishes any domain events that the aggregate produced during the current operation. The repository ensures that aggregate persistence and event publication occur atomically, either both succeed or both fail. This guarantees that domain events accurately reflect the state of the persisted aggregates.

## Technology Independence

Repository interfaces use domain types exclusively. They accept and return aggregate roots, entities, and value objects. They never expose database-specific types, query languages, or connection details. An implementation might use a relational database, a document store, or an event store, but the domain model is unaware of this choice and unaffected by technology changes.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6.
- Fowler, Martin. Patterns of Enterprise Application Architecture. Addison-Wesley, 2002.
