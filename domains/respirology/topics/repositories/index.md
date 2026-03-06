# Repositories in the Respirology Domain

## Overview

A repository is an abstraction that provides the illusion of an in-memory collection of aggregate roots. Repositories encapsulate the logic for storing, retrieving, and searching for aggregates, hiding the underlying persistence mechanism from the domain model. In Domain-Driven Design, repositories operate exclusively on aggregate roots -- each aggregate root has its own repository, and access to objects within the aggregate is always mediated through the root.

In the respirology domain, repositories provide clean interfaces for accessing clinical aggregates such as Respiratory Assessments, Diagnostic Orders, Care Plans, Ventilator Configurations, and Rehabilitation Programs. The domain model interacts with repositories using domain terminology, not database concepts.

## Repository Principles

### Aggregate Root Scope

Each repository manages a single aggregate root type. There is a RespiratoryAssessmentRepository, a DiagnosticOrderRepository, a CarePlanRepository, and so on. There is no generic "save anything" repository. This constraint ensures that aggregates are loaded and saved as complete units, preserving their invariants.

### Collection Semantics

A repository behaves like a collection. It supports operations analogous to adding an item to a collection, removing an item, and finding items by criteria. The repository does not expose SQL queries, connection strings, or storage details to the domain model.

### Domain-Oriented Query Methods

Repository query methods use domain terminology. Instead of `findByColumn("status", "active")`, a repository exposes `findActiveCarePlansByPatientId(patientId)`. This keeps the domain model expressive and prevents persistence concerns from leaking into domain logic.

## Repositories by Bounded Context

### Clinical Assessment Context

**RespiratoryAssessmentRepository**: Manages Respiratory Assessment aggregates. Key operations include saving a new or updated assessment, retrieving an assessment by its ID, finding all assessments for a given patient, finding assessments by encounter date range, and finding incomplete assessments requiring follow-up. Query methods such as `findByPatientIdAndDateRange(patientId, startDate, endDate)` and `findIncompleteAssessments()` support clinical workflows.

### Respiratory Diagnostics Context

**DiagnosticOrderRepository**: Manages Diagnostic Order aggregates. Key operations include creating new orders, updating order status, retrieving orders by ID, finding pending orders for a patient, and finding orders by test type. Query methods such as `findPendingOrdersByPatientId(patientId)` and `findOrdersByTestType(testType)` support diagnostic workflow management.

**DiagnosticReportRepository**: Manages Diagnostic Report aggregates. Key operations include saving finalized reports, retrieving reports by ID, finding reports by patient and date range, and finding reports with abnormal findings. Query methods such as `findByPatientIdAndTestType(patientId, testType)` support longitudinal result review.

### Airway Management Context

**AirwayProcedureRepository**: Manages Airway Procedure aggregates. Key operations include recording new procedures, retrieving procedures by ID, finding procedures for a patient, and finding recent procedures by type. Query methods such as `findByPatientIdAndProcedureType(patientId, procedureType)` support procedural history review.

### Chronic Disease Management Context

**CarePlanRepository**: Manages Care Plan aggregates. Key operations include creating and updating care plans, retrieving a plan by ID, finding the active care plan for a patient, finding care plans due for review, and finding care plans by disease type. Query methods such as `findActiveByPatientId(patientId)` and `findDueForReview(reviewDate)` support disease management workflows.

### Ventilatory Support Context

**VentilatorConfigurationRepository**: Manages Ventilator Configuration aggregates. Key operations include saving configurations, retrieving the current configuration for a patient, finding configurations with active alarms, and retrieving configuration history. Query methods such as `findCurrentByPatientId(patientId)` and `findWithActiveAlarms()` support ventilator management.

### Pulmonary Rehabilitation Context

**RehabilitationProgramRepository**: Manages Rehabilitation Program aggregates. Key operations include enrolling patients in programs, updating program details, finding active programs, finding programs by completion status, and retrieving program outcomes. Query methods such as `findActiveByPatientId(patientId)` and `findProgramsCompletedInDateRange(startDate, endDate)` support program management and outcome reporting.

## Repository Design Guidelines

### Interface Definition in the Domain Layer

Repository interfaces are defined in the domain layer, using domain types for parameters and return values. The implementation resides in the infrastructure layer, where specific persistence technologies (relational databases, document stores, event stores) are used. This separation ensures that the domain model is independent of infrastructure choices.

### No Business Logic in Repositories

Repositories perform persistence operations only. Business logic belongs in aggregates, entities, and domain services. A repository should not calculate a GOLD classification -- it should retrieve a Care Plan aggregate, which contains the classification logic.

### Transaction Boundaries

Each repository operation typically corresponds to a single transaction. Loading an aggregate, modifying it through domain methods, and saving it back constitutes a single unit of work. Cross-aggregate transactions are avoided; eventual consistency through domain events is preferred.

### Optimistic Concurrency

Repositories in the respirology domain should support optimistic concurrency to prevent conflicting updates. When two clinicians simultaneously update the same care plan, the repository detects the conflict and rejects the second update, allowing the application to resolve the conflict through domain logic.

### Read Model Separation

For complex query requirements that do not fit neatly into aggregate-based repositories, the respirology domain uses CQRS with separate read models. Read-optimized query services provide views such as patient respiratory dashboards, ventilator status boards, and rehabilitation outcome reports without loading full aggregates.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 6 on repositories.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 12 on repository implementations.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 10 on persistence and repository patterns.
