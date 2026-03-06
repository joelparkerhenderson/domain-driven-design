# Repositories in the Hormone Replacement Therapy Domain

Repositories provide an abstraction for storing and retrieving aggregate roots, presenting a collection-like interface that hides the underlying persistence mechanism. In the hormone replacement therapy (HRT) domain, repositories serve each bounded context by managing the lifecycle of its aggregate roots while keeping domain logic free of infrastructure concerns.

## Purpose

HRT systems must persist clinical assessments, treatment protocols, monitoring plans, adverse event records, optimization recommendations, and treatment outcomes. Without repositories, persistence logic would leak into the domain model, coupling clinical business rules to specific database technologies. Repositories isolate this concern by providing domain-oriented interfaces that the domain model uses to store and retrieve aggregates, while the implementation details remain in the infrastructure layer.

## Repository Design Principles

Repositories in the HRT domain follow several core principles. Each repository manages exactly one aggregate root type. Repository interfaces are defined in the domain layer using domain terminology. Repository implementations reside in the infrastructure layer and are injected through dependency inversion. Repositories return fully reconstituted aggregates, not partial projections or raw data. Query methods use domain-meaningful parameters rather than generic query languages.

## Clinical Assessment Context Repositories

### ClinicalAssessmentRepository

Manages persistence of ClinicalAssessment aggregates. Key operations include saving a new assessment, retrieving an assessment by AssessmentId, finding assessments by PatientId, and querying assessments by candidacy status. The repository ensures that a retrieved ClinicalAssessment includes its complete internal structure: symptom evaluations, hormone panel interpretations, and risk factor profiles. It supports retrieval of the most recent assessment for a given patient to support clinical workflow continuity.

### PatientProfileRepository

Manages persistence of PatientProfile aggregates. Key operations include saving and retrieving patient profiles by PatientId, and querying profiles by demographic or clinical criteria relevant to cohort analysis. The repository enforces that each patient has exactly one active profile, preventing duplicate records.

## Hormone Protocol Context Repositories

### TreatmentProtocolRepository

Manages persistence of TreatmentProtocol aggregates. Key operations include saving a new protocol, retrieving a protocol by ProtocolId, finding active protocols by PatientId, and querying protocols by status. The repository returns the complete protocol including all ProtocolComponents with their formulations, dosages, and schedules. It supports version history retrieval to track protocol modifications over time, which is essential for audit trails and clinical review.

### FormulationCatalogRepository

Manages persistence of FormulationCatalog aggregates. Key operations include retrieving the current catalog, querying formulations by hormone type, delivery method, or regulatory approval status, and updating catalog entries when formulation availability changes. The repository supports point-in-time retrieval to determine which formulations were available when a specific protocol was prescribed.

## Lab Monitoring Context Repositories

### MonitoringPlanRepository

Manages persistence of MonitoringPlan aggregates. Key operations include saving a new monitoring plan, retrieving a plan by MonitoringPlanId, finding active plans by PatientId, and querying plans with overdue scheduled tests. The repository returns the complete plan including all scheduled tests and recorded results. It supports temporal queries to find plans requiring attention within a specified time window, enabling proactive monitoring management.

### LabResultSetRepository

Manages persistence of LabResultSet aggregates. Key operations include saving a new result set, retrieving result sets by PatientId and date range, and querying results by test type for trend analysis. The repository supports efficient retrieval of sequential results for a specific test type, which is critical for the trend analysis that drives treatment optimization.

## Side Effect Management Context Repositories

### AdverseEventRecordRepository

Manages persistence of AdverseEventRecord aggregates. Key operations include saving a new adverse event record, retrieving records by EventId, finding records by PatientId, querying by severity grade, and finding unresolved events. The repository returns the complete record including causality assessments and mitigation actions. It supports querying by event classification to identify patterns across patients, supporting pharmacovigilance signal detection.

### ContraindicationRegistryRepository

Manages persistence of ContraindicationRegistry aggregates. Key operations include retrieving the current registry, querying contraindications by hormone type, and updating the registry when new clinical evidence changes contraindication classifications. The repository ensures that the registry is always retrieved as a complete, consistent unit since partial contraindication data could lead to unsafe clinical decisions.

## Treatment Optimization Context Repositories

### OptimizationRecommendationRepository

Manages persistence of OptimizationRecommendation aggregates. Key operations include saving a new recommendation, retrieving recommendations by RecommendationId, finding recommendations by ProtocolId, and querying by recommendation status. The repository supports retrieval of recommendation history for a given protocol to track the optimization trajectory and evaluate the effectiveness of previous adjustments.

## Outcomes Tracking Context Repositories

### TreatmentOutcomeRepository

Manages persistence of TreatmentOutcome aggregates. Key operations include saving and updating treatment outcomes, retrieving outcomes by OutcomeId, finding outcomes by PatientId, and querying by efficacy rating for population-level analysis. The repository supports efficient retrieval of longitudinal measurement data, which can span months or years of treatment history. It provides methods for retrieving outcome data within specified date ranges to support periodic outcome assessments.

## Query Considerations

While repositories primarily serve command-side operations (storing and retrieving aggregates for modification), the HRT domain also requires read-optimized queries for clinical dashboards, reporting, and population analysis. Following CQRS principles, complex read queries may be served by dedicated read models rather than through repository interfaces, keeping repositories focused on aggregate lifecycle management.

## Transactional Boundaries

Each repository operation aligns with the aggregate transactional boundary. A single repository save operation persists the entire aggregate atomically. Cross-aggregate consistency is achieved through domain events and eventual consistency rather than through multi-aggregate transactions spanning multiple repositories.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on implementing business logic.
- Fowler, Martin. Patterns of Enterprise Application Architecture. Addison-Wesley, 2002.
