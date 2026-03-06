# Repositories in Ophthalmology

## Overview

Repositories provide an abstraction for storing and retrieving aggregate roots from
persistence mechanisms. In Domain-Driven Design, repositories present a collection-like
interface to the domain layer, hiding the details of data storage and retrieval. In the
ophthalmology domain, repositories manage the persistence of clinical aggregates such
as patient examinations, imaging studies, surgical cases, and treatment plans while
keeping the domain model free of infrastructure concerns.

## Repository Design Principles

Repositories in the ophthalmology domain follow these principles:

- Repositories operate on aggregate roots only. There is no repository for individual
  entities or value objects within an aggregate; those are persisted as part of their
  containing aggregate.
- Repository interfaces are defined in the domain layer using domain terminology. The
  implementation resides in an infrastructure layer.
- Repositories return fully reconstituted aggregates. When an aggregate is retrieved,
  all its internal entities and value objects are loaded and the aggregate's invariants
  are satisfied.
- Repositories support domain-meaningful queries. Instead of generic SQL-like query
  interfaces, repositories expose methods named using the ubiquitous language.

## Repositories by Bounded Context

### Clinical Examination Context

**PatientExaminationRepository**: Manages persistence of Patient Examination aggregates.
Key operations include finding examinations by patient ID, retrieving examinations
within a date range, finding the most recent examination for a given patient and eye
laterality, and querying examinations with specific finding types (e.g., elevated IOP,
abnormal fundus findings).

**RefractionResultRepository**: Manages persistence of Refraction Result aggregates.
Key operations include retrieving refraction history for a patient, finding the most
recent refraction, and querying refractions by prescription characteristics.

### Diagnostic Imaging Context

**ImagingStudyRepository**: Manages persistence of Imaging Study aggregates. Key
operations include finding studies by patient and modality, retrieving studies by
clinical indication, querying studies by interpretation status (uninterpreted,
preliminary, finalized), and finding studies within a date range for a specific
imaging modality.

**OCTScanRepository**: Manages persistence of OCT Scan aggregates. Key operations
include retrieving OCT scans for a specific patient and eye, finding scans by scan
protocol type, and querying for scans with measurements outside normative ranges.

**VisualFieldTestRepository**: Manages persistence of Visual Field Test aggregates.
Key operations include retrieving visual field test series for progression analysis,
finding tests by reliability criteria, and querying for tests showing statistically
significant deterioration.

### Surgical Management Context

**SurgicalCaseRepository**: Manages persistence of Surgical Case aggregates. Key
operations include finding cases by surgeon and date, retrieving cases by procedure
type, querying cases by status (scheduled, in-progress, completed, cancelled), and
finding cases with specific complications for quality review.

**OperativePlanRepository**: Manages persistence of Operative Plan aggregates. Key
operations include retrieving plans by procedure type and implant specifications,
and finding plans pending review or approval.

### Refractive Services Context

**RefractiveCandidateAssessmentRepository**: Manages persistence of candidate
assessment aggregates. Key operations include finding assessments by candidacy status,
retrieving assessments for patients meeting specific corneal criteria, and querying
assessments pending final determination.

**RefractiveProcedureRepository**: Manages persistence of refractive procedure
aggregates. Key operations include retrieving procedures by type and outcome, and
finding procedures within follow-up windows.

### Glaucoma Management Context

**GlaucomaPatientProfileRepository**: Manages persistence of longitudinal glaucoma
profiles. Key operations include retrieving profiles by progression status, finding
profiles requiring treatment plan review, querying profiles by current medication
regimen, and retrieving profiles with IOP above target.

**TreatmentPlanRepository**: Manages persistence of glaucoma treatment plans. Key
operations include finding active treatment plans by medication class, retrieving
plans due for reassessment, and querying plans by treatment response.

### Retinal Care Context

**RetinalAssessmentRepository**: Manages persistence of retinal assessment aggregates.
Key operations include finding assessments by condition type and severity, retrieving
assessments for patients due for follow-up, and querying assessments showing treatment
response changes.

**InjectionProtocolRepository**: Manages persistence of injection protocol aggregates.
Key operations include finding protocols by medication and treatment regimen, retrieving
protocols with upcoming scheduled injections, and querying protocols by treatment
response patterns.

## Query Patterns

Repositories support two categories of operations aligned with CQRS principles:

- Command-side operations: add, save, and remove aggregates. These operations enforce
  aggregate invariants and publish domain events.
- Query-side operations: find and retrieve aggregates by domain-meaningful criteria.
  Complex analytical queries (e.g., population-level progression analysis) may be
  served by dedicated read models rather than the aggregate repository.

## Repository Implementation Considerations

- Repositories are the only mechanism for obtaining aggregate references. Domain
  services and application services access aggregates exclusively through repositories.
- Repositories handle optimistic concurrency. Clinical data requires concurrency
  control to prevent lost updates when multiple clinicians access the same patient
  record.
- Repositories support unit-of-work patterns to coordinate aggregate persistence
  within a single transaction boundary.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5.
