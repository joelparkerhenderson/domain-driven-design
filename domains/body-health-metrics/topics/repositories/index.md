# Repositories for Body Health Metrics

## Overview

Repositories in the body health metrics domain provide abstractions for storing and retrieving aggregate roots. They encapsulate the mechanisms of persistence, presenting a collection-like interface to the domain model. The domain model interacts with repositories as if aggregates were stored in an in-memory collection, without awareness of underlying storage technologies, query optimization, or data serialization. This separation ensures that body health business logic remains pure and independent of infrastructure concerns.

## Key Repositories

### MeasurementSessionRepository

**Context**: Biometric Collection

Manages the persistence and retrieval of MeasurementSession aggregates, the primary data capture records in the body health metrics system.

- **Store operations**: save a new measurement session; update a session in progress
- **Retrieval operations**: find by MeasurementSessionId; find by PersonId and date range; find most recent session for a person; find sessions by measurement type
- **Domain queries**: retrieve all sessions containing a specific measurement type for a given person within a time window; find sessions recorded by a specific device
- **Aggregate boundary**: Returns complete MeasurementSession aggregates with all child measurement records

### CompositionProfileRepository

**Context**: Body Composition

Manages persistence of CompositionProfile aggregates representing body composition analyses.

- **Store operations**: save a new composition profile; update an existing profile with revised analysis
- **Retrieval operations**: find by CompositionProfileId; find by PersonId and date range; find most recent profile for a person
- **Domain queries**: retrieve composition history for trend analysis; find profiles by analysis method (BIA, DEXA, calipers)
- **Aggregate boundary**: Returns complete CompositionProfile aggregates including all component value objects (fat mass, lean mass, bone density, hydration)

### FitnessAssessmentRepository

**Context**: Fitness Assessment

Manages persistence of FitnessAssessment aggregates representing fitness evaluation results.

- **Store operations**: save a completed fitness assessment
- **Retrieval operations**: find by FitnessAssessmentId; find by PersonId and assessment type; find most recent assessment for a person
- **Domain queries**: retrieve assessment history for a specific fitness component (cardiorespiratory, strength, flexibility); find assessments by protocol type
- **Aggregate boundary**: Returns complete FitnessAssessment aggregates with all test results and normative comparisons

### HealthScoreRepository

**Context**: Health Scoring

Manages persistence of HealthScore aggregates representing composite health assessments.

- **Store operations**: save a new health score calculation
- **Retrieval operations**: find by HealthScoreId; find by PersonId and date range; find most recent score for a person
- **Domain queries**: retrieve score history for trend analysis; find scores by scoring algorithm version; retrieve component score breakdowns
- **Aggregate boundary**: Returns complete HealthScore aggregates including component scores and normative comparisons

### HealthGoalRepository

**Context**: Progress Tracking

Manages persistence of HealthGoal aggregates representing health improvement objectives.

- **Store operations**: save a new health goal; update goal progress; mark goal as completed or abandoned
- **Retrieval operations**: find by HealthGoalId; find active goals for a person; find completed goals for a person
- **Domain queries**: retrieve goals by target metric type; find goals approaching their target date; find goals with stalled progress
- **Aggregate boundary**: Returns complete HealthGoal aggregates with milestone definitions and progress checkpoints

### ClinicalReportRepository

**Context**: Clinical Integration

Manages persistence of ClinicalReport aggregates representing formatted health data packages for clinical systems.

- **Store operations**: save a new clinical report; update submission status
- **Retrieval operations**: find by ClinicalReportId; find by PersonId and date range; find reports by submission status
- **Domain queries**: retrieve reports pending submission; find reports by destination system; find reports requiring resubmission
- **Aggregate boundary**: Returns complete ClinicalReport aggregates with all clinical observations and coding

## Repository Design Principles

**Aggregate root access only**: Repositories provide access to aggregate roots exclusively. Individual measurements within a MeasurementSession are accessed through the session aggregate, never directly through a repository. This preserves aggregate invariants and consistency boundaries.

**Collection semantics**: Repository interfaces present collection-like semantics (add, find, remove) rather than database semantics (insert, select, delete). The domain model treats the repository as a domain concept, not a data access layer.

**Domain-oriented queries**: Repository query methods express domain concepts, not data storage concepts. A method named findRecentMeasurementsForPerson(PersonId, DateRange) communicates domain intent, while findByColumns(Map) does not.

**No query logic leakage**: Complex filtering, sorting, and pagination logic does not leak into the domain model. Repository implementations handle query optimization internally. The domain model specifies what data it needs; the repository determines how to retrieve it efficiently.

**Unit of work**: Repositories participate in the unit of work pattern, ensuring that changes to aggregates within a single business operation are persisted atomically. When a MeasurementSession is completed and a domain event is published, both the aggregate state change and the event are persisted together.

**Read model separation**: For complex query requirements (such as cross-aggregate reporting or population-level analytics), separate read models may be maintained alongside the repository. The repository serves the write model; dedicated query services serve the read model following CQRS principles.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on persistence patterns.
