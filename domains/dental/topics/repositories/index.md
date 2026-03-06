# Repositories - Dental Domain

## Overview

Repositories provide abstractions for storing and retrieving aggregate roots, shielding the domain model from infrastructure concerns such as database technology, query mechanisms, and persistence formats. In the dental domain, repositories serve as the interface through which bounded contexts access their aggregate roots, whether retrieving a patient's clinical record for examination updates or loading a treatment plan for modification. Repositories express their operations in terms of the ubiquitous language, not in terms of database operations.

## Repository Design Principles in Dental Systems

Dental repositories must support the clinical workflow patterns characteristic of dental practice. Dental professionals frequently need to access a patient's complete clinical record during an appointment, load treatment plans for review during case presentations, and query appointment schedules by date and provider. Repositories provide these access patterns as domain-meaningful operations rather than generic data access methods.

Repositories operate at the aggregate root level. There is no repository for individual tooth entities because teeth exist within the Patient Clinical Record aggregate. Accessing a tooth's data requires loading the Patient Clinical Record aggregate through its repository.

## Repositories by Bounded Context

### Clinical Examination Context

**PatientClinicalRecordRepository**
Provides access to Patient Clinical Record aggregates. Key operations include:
- Retrieve a patient's clinical record by patient identifier.
- Find patients with examination findings matching specific condition criteria.
- Find patients due for periodic examination based on last examination date.
- Persist updated clinical records after examination findings are recorded.

**RadiographicSeriesRepository**
Provides access to Radiographic Series aggregates. Key operations include:
- Retrieve a radiographic series by series identifier.
- Find all radiographic series for a patient ordered by date.
- Find patients due for radiographic imaging based on imaging schedule and risk assessment.
- Persist new radiographic series records.

### Treatment Planning Context

**TreatmentPlanRepository**
Provides access to Treatment Plan aggregates. Key operations include:
- Retrieve a treatment plan by plan identifier.
- Find all treatment plans for a patient, optionally filtered by status (draft, presented, approved, completed).
- Find treatment plans with pending consent requirements.
- Persist new or modified treatment plans.

**CasePresentationRepository**
Provides access to Case Presentation aggregates. Key operations include:
- Retrieve a case presentation by presentation identifier.
- Find case presentations for a specific treatment plan.
- Persist new case presentation materials.

### Restorative Services Context

**RestorativeCaseRepository**
Provides access to Restorative Case aggregates. Key operations include:
- Retrieve a restorative case by case identifier.
- Find active restorative cases for a patient with pending stages.
- Find restorative cases by procedure type or material for outcome analysis.
- Persist restorative case updates as procedures progress through stages.

**EndodonticCaseRepository**
Provides access to Endodontic Case aggregates. Key operations include:
- Retrieve an endodontic case by case identifier.
- Find endodontic cases for a patient.
- Find cases with pending obturation (started but not completed).
- Persist endodontic case progress updates.

### Orthodontic Management Context

**OrthodonticCaseRepository**
Provides access to Orthodontic Case aggregates. Key operations include:
- Retrieve an orthodontic case by case identifier.
- Find active orthodontic cases for a provider.
- Find cases approaching estimated completion dates.
- Find cases in retention phase with upcoming retainer check appointments.
- Persist orthodontic case updates including adjustment visit records.

### Periodontal Care Context

**PeriodontalRecordRepository**
Provides access to Periodontal Record aggregates. Key operations include:
- Retrieve a periodontal record by patient identifier.
- Find patients with specific disease stage or grade classifications.
- Find patients due for periodontal maintenance based on prescribed intervals.
- Find patients with probing depths exceeding specified thresholds.
- Persist updated periodontal records after assessments or treatments.

### Practice Management Context

**PatientAccountRepository**
Provides access to Patient Account aggregates. Key operations include:
- Retrieve a patient account by patient identifier.
- Find patient accounts with outstanding balances.
- Find patient accounts due for recall by date range.
- Persist patient account updates for scheduling and financial changes.

**InsuranceClaimRepository**
Provides access to Insurance Claim aggregates. Key operations include:
- Retrieve a claim by claim identifier.
- Find claims by status (draft, submitted, pending, paid, denied).
- Find claims for a patient within a date range.
- Find claims approaching timely filing deadlines.
- Persist claim lifecycle updates.

## Repository Implementation Guidelines

Repositories in the dental domain follow these guidelines:

1. Repository interfaces are defined in the domain layer using ubiquitous language terminology.
2. Repository implementations reside in the infrastructure layer and are injected into domain services.
3. Query methods return fully reconstituted aggregates, not partial projections. Read-specific queries belong to the query side under CQRS.
4. Repositories do not contain business logic; they delegate all validation to the aggregate root.
5. Collection-oriented repositories model aggregate persistence as an in-memory collection, while persistence-oriented repositories model explicit save operations.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12 on repository implementations.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on persistence patterns.
