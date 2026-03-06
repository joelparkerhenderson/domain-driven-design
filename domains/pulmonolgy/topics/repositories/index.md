# Repositories in the Pulmonolgy Domain

## Overview

Repositories provide abstractions for storing and retrieving aggregate roots. They encapsulate the logic needed to access data sources, presenting a collection-like interface to the domain model while hiding the details of data storage and retrieval. In the pulmonolgy domain, repositories serve as the gateway between the domain model and persistence infrastructure, ensuring that clinical data access is expressed in domain terms.

Repositories operate exclusively on aggregate roots. You retrieve a PatientAssessment aggregate through its repository, not individual SymptomEvaluation records. This constraint reinforces aggregate boundaries and ensures that consistency rules are maintained whenever data is loaded or saved.

## Repositories by Bounded Context

### Pulmonary Assessment Context

**PatientAssessmentRepository**: Provides access to PatientAssessment aggregates. Supports retrieval by assessment identifier, by patient identifier (returning all assessments for a patient), and by date range. Supports queries for incomplete assessments requiring follow-up and for assessments with abnormal findings.

**PFTSessionRepository**: Provides access to PFTSession aggregates. Supports retrieval by session identifier and by patient identifier with date filtering. Enables queries for sessions requiring quality review and for trending PFT results over time for a given patient.

### Respiratory Diagnostics Context

**DiagnosticProcedureRepository**: Provides access to DiagnosticProcedure aggregates. Supports retrieval by procedure identifier, by patient, and by procedure type. Enables queries for procedures in specific lifecycle states (scheduled, awaiting consent, completed, awaiting results) and for procedures with documented complications.

**DiagnosticResultRepository**: Provides access to DiagnosticResult aggregates. Supports retrieval by result identifier, by linked procedure, and by patient. Enables queries for results pending clinician review and for results matching specific diagnostic classifications.

### Chronic Disease Management Context

**TreatmentPlanRepository**: Provides access to TreatmentPlan aggregates. Supports retrieval by plan identifier, by patient and disease type, and by plan status (active, suspended, superseded). Enables queries for plans due for scheduled review and for plans with recent medication changes requiring follow-up.

**ExacerbationRecordRepository**: Provides access to ExacerbationRecord aggregates. Supports retrieval by record identifier and by patient with date filtering. Enables queries for active exacerbations requiring monitoring, for exacerbation frequency analysis over specified time periods, and for exacerbations by severity classification.

**ChronicDiseaseRecordRepository**: Provides access to ChronicDiseaseRecord entities at the aggregate root level. Supports retrieval by patient and disease type. Enables queries for patients with progressive disease requiring intervention and for patients due for severity reclassification assessment.

### Critical Care Context

**VentilationSessionRepository**: Provides access to VentilationSession aggregates. Supports retrieval by session identifier and by patient. Enables queries for active ventilation sessions, for sessions exceeding defined duration thresholds, and for sessions with active weaning protocols.

**ARDSManagementRepository**: Provides access to ARDSManagement aggregates. Supports retrieval by episode identifier and by patient. Enables queries for active ARDS episodes by severity classification and for episodes requiring reassessment based on clinical trajectory.

### Sleep Medicine Context

**SleepStudyRepository**: Provides access to SleepStudy aggregates. Supports retrieval by study identifier and by patient. Enables queries for studies awaiting scoring, for studies awaiting physician interpretation, and for studies by diagnostic classification and severity.

**TherapyManagementRepository**: Provides access to TherapyManagement aggregates. Supports retrieval by therapy identifier and by patient. Enables queries for patients with suboptimal compliance, for prescriptions requiring renewal, and for patients needing pressure titration reassessment.

### Pulmonary Rehabilitation Context

**RehabilitationProgramRepository**: Provides access to RehabilitationProgram aggregates. Supports retrieval by program identifier and by patient. Enables queries for active enrollments, for programs nearing completion, and for patients with attendance below minimum thresholds.

**ProgressAssessmentRepository**: Provides access to ProgressAssessment aggregates. Supports retrieval by assessment identifier and by patient with date filtering. Enables queries for assessments showing significant improvement or decline, and for patients due for scheduled progress evaluation.

## Repository Design Principles

Repositories in the pulmonolgy domain follow these design principles:

- **Aggregate Root Focus**: Repositories exist only for aggregate roots. There is no repository for SymptomEvaluation or VentilatorSettings because these are internal to their respective aggregates and are accessed through the aggregate root.

- **Domain Language Queries**: Repository query methods use domain terminology. A method named "findPatientsWithSuboptimalCompliance" is preferred over a generic "findByComplianceLessThan" because it expresses a clinical concept.

- **Collection Semantics**: Repositories present a collection-like interface. Adding a new aggregate is expressed as adding to the collection. Retrieving an aggregate is expressed as finding within the collection. The underlying persistence mechanism is not exposed.

- **Reconstitution**: When retrieving an aggregate, the repository reconstitutes the complete aggregate including all internal entities and value objects. The domain model receives a fully formed aggregate, not a partially loaded object requiring lazy loading.

- **Specification Pattern**: Complex queries can use the specification pattern to compose clinical criteria. For example, a specification for "COPD patients with two or more exacerbations in the past year and declining FEV1" combines multiple clinical criteria into a reusable query object.

## Repository and CQRS

In contexts where read and write patterns differ significantly, repositories may be complemented by separate read models following the CQRS pattern. The Critical Care Context, for example, may maintain a read-optimized view of current ventilation status across all ICU patients that is separate from the write-optimized VentilationSession repository.

Read models are particularly valuable in the Chronic Disease Management Context for population health analytics, where aggregate-level queries (disease registry reports, exacerbation frequency trends, treatment plan compliance summaries) require different data structures than transactional write operations.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories as abstractions for aggregate root persistence.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repository implementation strategies and patterns.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on repository patterns and their relationship to aggregates.
