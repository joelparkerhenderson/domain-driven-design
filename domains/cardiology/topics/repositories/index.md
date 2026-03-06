# Repositories in Cardiology

## Overview

Repositories are abstractions that encapsulate the logic for storing and retrieving aggregate roots. In the cardiology domain, repositories provide a collection-like interface for accessing clinical aggregates without exposing the underlying persistence mechanism. This separation ensures that cardiology business logic remains pure and independent of database technology, allowing domain experts and developers to reason about clinical workflows without infrastructure concerns.

## Repository Principles for Cardiology

Cardiology repositories follow core DDD principles adapted to clinical requirements:

- **Aggregate root access only**: Repositories provide access to aggregate roots, not internal entities or value objects. To access a coronary lesion, one retrieves the CatheterizationProcedure aggregate root and navigates to the lesion within it.
- **Collection semantics**: Repositories behave as in-memory collections of aggregates. They support operations like add, remove, and find by criteria.
- **Domain-oriented query methods**: Repository methods use ubiquitous language. A method named "findPatientsWithUnoptimizedGDMT" is preferred over "findByMedicationStatusNotComplete."
- **Infrastructure independence**: Repository interfaces are defined in the domain layer. Implementations reside in the infrastructure layer and may use SQL databases, document stores, or any other technology.

## Repository Definitions by Bounded Context

### Clinical Assessment Context

**PatientAssessmentRepository**: Manages PatientAssessment aggregates. Key operations include finding assessments by patient identifier, retrieving assessments within a date range, finding incomplete assessments requiring follow-up, and querying assessments by risk score category. Domain-specific queries support clinical workflows such as identifying patients due for reassessment or those with elevated risk scores requiring intervention.

**RiskStratificationRepository**: Manages RiskStratification aggregates. Supports retrieval by patient identifier and score type, querying for patients within specific risk categories, and tracking risk score trends over time. Enables population-level risk analysis for quality reporting.

**StressTestRepository**: Manages StressTest aggregates. Supports retrieval by patient identifier and test date, finding tests by result category (positive, negative, equivocal), and identifying tests pending final interpretation.

### Diagnostic Imaging Context

**ImagingStudyRepository**: Manages ImagingStudy aggregates. Key operations include finding studies by accession number, retrieving studies by patient and modality, querying for studies pending interpretation, and finding studies with critical findings. Supports imaging workflow management including worklist generation for reading physicians.

**ImagingOrderRepository**: Manages ImagingOrder aggregates. Supports finding unfulfilled orders, orders by clinical indication, and orders requiring prior authorization. Enables order tracking and scheduling coordination.

### Interventional Procedures Context

**CatheterizationProcedureRepository**: Manages CatheterizationProcedure aggregates. Supports retrieval by procedure identifier, finding procedures by patient, querying for procedures with specific complications, and retrieving procedures by operator for credentialing purposes. Enables outcomes tracking and quality assurance reporting.

**DeviceImplantationRepository**: Manages DeviceImplantation aggregates. Supports finding implantations by device serial number, querying by device type and model for recall management, and tracking device outcomes by implantation site and operator.

### Electrophysiology Context

**EPStudyRepository**: Manages EPStudy aggregates. Supports retrieval by study identifier, finding studies by arrhythmia type, and querying for studies with inducible arrhythmias requiring intervention.

**AblationProcedureRepository**: Manages AblationProcedure aggregates. Supports finding procedures by target arrhythmia type, querying acute success rates, and tracking long-term recurrence for quality reporting.

**CIEDImplantationRepository**: Manages CIEDImplantation aggregates. Supports finding devices by serial number, querying devices by manufacturer and model for advisory or recall management, finding devices approaching battery end-of-life, and retrieving all devices for a specific patient across their implant history.

### Heart Failure Management Context

**HeartFailureProfileRepository**: Manages HeartFailureProfile aggregates. Supports finding profiles by patient identifier, querying by heart failure phenotype (HFrEF, HFmrEF, HFpEF), filtering by NYHA class and ACC/AHA stage, and identifying patients with recent clinical deterioration. Critical for population health management of heart failure cohorts.

**GDMTRegimenRepository**: Manages GDMTRegimen aggregates. Supports finding regimens by patient, querying for regimens not at target doses, identifying patients eligible for medication initiation or uptitration, and tracking time-to-optimization metrics. Enables GDMT quality improvement initiatives.

### Cardiac Rehabilitation Context

**RehabilitationProgramRepository**: Manages RehabilitationProgram aggregates. Supports finding programs by patient, querying active programs by phase, identifying patients with incomplete programs, and tracking program completion rates by qualifying diagnosis.

**RehabilitationSessionRepository**: Manages RehabilitationSession aggregates. Supports finding sessions by patient and date range, querying session attendance rates, tracking exercise capacity trends, and identifying sessions with adverse events.

## Query Specifications

Complex repository queries use the specification pattern to compose domain-meaningful criteria. For example, a specification for "patients eligible for cardiac rehabilitation referral" might compose criteria for recent MI, recent PCI, recent CABG, or heart failure with NYHA class II-III, excluding patients with active contraindications to exercise.

## Read Model Considerations

For complex reporting and analytics queries (population-level risk reports, quality measure dashboards, device registry submissions), CQRS principles suggest maintaining separate read models optimized for these query patterns, rather than forcing complex queries through aggregate-oriented repositories.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on repository patterns.
- ACC NCDR Registry Data Standards for cardiovascular data collection and reporting.
