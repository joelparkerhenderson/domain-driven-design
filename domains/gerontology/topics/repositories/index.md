# Repositories in the Gerontology Domain

## Overview

Repositories provide an abstraction for storing and retrieving aggregate roots, hiding the details of the underlying persistence mechanism from the domain model. In the gerontology domain, repositories serve as the boundary between the domain layer and the infrastructure layer, ensuring that business logic remains pure and independent of database technology, file systems, or external storage services.

Evans (2003) describes repositories as providing the illusion of an in-memory collection of domain objects. Vernon (2013) emphasizes that repositories should only exist for aggregate roots, not for individual entities or value objects within an aggregate. In geriatric care systems, repositories manage the lifecycle of clinical assessments, care plans, medication regimens, and other aggregate roots that must persist across sessions and care episodes.

## Repository Design Principles

Repositories in the gerontology domain follow several design principles. Each repository corresponds to exactly one aggregate root type. Repository interfaces are defined in the domain layer and use domain language, not persistence language. Implementation details (SQL queries, document storage, API calls) belong to the infrastructure layer. Repositories return fully reconstituted aggregates, not partial objects or raw data.

Queries exposed by repositories reflect domain concepts rather than technical queries. A method named findPatientsWithHighFallRisk is preferred over a generic findByRiskLevel method. This approach keeps the domain language consistent throughout the codebase.

## Geriatric Assessment Context Repositories

### GeriatricAssessmentRepository

Manages the persistence of GeriatricAssessment aggregates. Provides methods to save a new assessment, retrieve an assessment by its identifier, find all assessments for a given patient, find assessments by date range, and find incomplete assessments requiring follow-up. Supports longitudinal queries that retrieve a patient's assessment history to enable trend analysis across multiple CGAs.

### FrailtyScreeningRepository

Manages the persistence of FrailtyScreening aggregates. Provides methods to save a screening result, retrieve screenings by patient, and find screenings where the frailty status changed from a previous evaluation. Supports queries that compare sequential frailty assessments to detect worsening or improvement.

## Care Coordination Context Repositories

### CarePlanRepository

Manages the persistence of CarePlan aggregates. Provides methods to save or update a care plan, retrieve the current active care plan for a patient, find care plans by status (active, completed, suspended), and retrieve care plan history for audit purposes. Supports versioned retrieval to compare how a care plan has evolved over time.

### CareTransitionRepository

Manages the persistence of CareTransition aggregates. Provides methods to save a transition record, find active transitions, and retrieve transitions for a patient within a date range. Supports queries that identify transitions lacking required components such as medication reconciliation or transfer summaries.

## Cognitive Health Context Repositories

### CognitiveScreeningRepository

Manages the persistence of CognitiveScreening aggregates. Provides methods to save screening results, retrieve screenings by patient and instrument type, and find patients whose cognitive scores have declined beyond a threshold between sequential assessments. Supports longitudinal queries essential for tracking cognitive trajectory.

### MemoryCarePlanRepository

Manages the persistence of MemoryCarePlan aggregates. Provides methods to save or update a memory care plan, retrieve the active plan for a patient, and find plans that require periodic review based on the patient's dementia stage progression schedule.

## Functional Independence Context Repositories

### FunctionalAssessmentRepository

Manages the persistence of FunctionalAssessment aggregates. Provides methods to save assessment results, retrieve assessments by patient, and find patients whose ADL or IADL scores have declined beyond a threshold. Supports comparison queries that identify functional decline patterns across assessment periods.

### HomeModificationPlanRepository

Manages the persistence of HomeModificationPlan aggregates. Provides methods to save modification plans, find plans with outstanding recommendations, and retrieve completed modifications for a patient's residence. Supports queries that track modification implementation rates and outstanding safety recommendations.

## Medication Management Context Repositories

### MedicationRegimenRepository

Manages the persistence of MedicationRegimen aggregates. Provides methods to save or update a regimen, retrieve the current regimen for a patient, and find patients with polypharmacy (five or more concurrent medications). Supports queries that identify patients taking Beers criteria medications, enabling proactive pharmacist review.

### DeprescribingPlanRepository

Manages the persistence of DeprescribingPlan aggregates. Provides methods to save deprescribing plans, find active tapering schedules, and retrieve plans by status (planned, in-progress, completed, abandoned). Supports queries that track deprescribing outcomes across the patient population.

## End of Life Planning Context Repositories

### AdvanceDirectiveRepository

Manages the persistence of AdvanceDirective aggregates. Provides methods to save or update directives, retrieve the current directive for a patient, and find patients without documented advance directives. Supports compliance queries that identify patients who should be offered advance care planning conversations.

### GoalsOfCareConversationRepository

Manages the persistence of GoalsOfCareConversation aggregates. Provides methods to save conversation records, retrieve conversations by patient, and find patients whose last goals-of-care conversation exceeds a specified age threshold. Supports queries that identify patients due for periodic preference reassessment.

## Cross-Cutting Repository Concerns

All repositories in the gerontology domain must support audit trail requirements. Every save and update operation records the user, timestamp, and reason for change. This supports regulatory compliance (HIPAA audit requirements) and clinical quality review.

Repositories must also support soft deletion for clinical data. Patient records are never physically deleted but may be marked as superseded, corrected, or withdrawn. This approach preserves the clinical audit trail and supports legal requirements for medical record retention.

Repository implementations must handle the concurrent access patterns common in interdisciplinary care teams, where multiple clinicians may be updating different aspects of a patient's care simultaneously. Optimistic concurrency control is preferred, raising domain-specific exceptions when conflicts are detected.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
