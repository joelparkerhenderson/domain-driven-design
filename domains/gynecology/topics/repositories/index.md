# Repositories in the Gynecology Domain

## Overview

A repository is an abstraction that provides the illusion of an in-memory collection of aggregate roots. Repositories encapsulate the logic for storing, retrieving, and searching for aggregates, hiding the underlying persistence mechanism from the domain model. In the gynecology domain, repositories serve as the boundary between domain logic and data storage, ensuring that clinical business rules remain independent of database technology.

## Purpose

Gynecology systems must persist clinical data reliably and retrieve it efficiently. Patient encounters, prenatal records, surgical cases, and screening episodes must be stored securely and retrieved accurately. Repositories provide a clean interface for these operations, allowing the domain model to express clinical logic without concern for how data is physically stored or queried.

## Repositories by Bounded Context

### Clinical Assessment Context

**ClinicalEncounterRepository**
- Retrieves encounters by encounter ID, patient ID, or date range.
- Supports queries for open encounters, encounters pending diagnostic workup results, and encounters requiring follow-up.
- Persists the complete Clinical Encounter aggregate including all contained value objects and child entities.

**DiagnosticWorkupRepository**
- Retrieves workups by workup ID, encounter ID, or status.
- Supports queries for workups with pending test results.
- Used when workup lifecycle management needs to operate independently of encounter queries.

### Reproductive Health Context

**ReproductiveHealthPlanRepository**
- Retrieves plans by plan ID or patient ID.
- Supports queries for active plans, plans requiring review, and plans with specific contraceptive methods.
- Persists the complete Reproductive Health Plan aggregate including fertility assessments, menstrual history, and treatment protocols.

### Surgical Services Context

**SurgicalCaseRepository**
- Retrieves cases by case ID, patient ID, surgeon ID, or scheduled date.
- Supports queries for cases by status (referred, consented, scheduled, completed, closed).
- Supports queries for cases requiring postoperative follow-up.
- Persists the complete Surgical Case aggregate including consent, operative plan, and follow-up records.

### Prenatal Care Context

**PrenatalRecordRepository**
- Retrieves records by record ID or patient ID.
- Supports queries for active pregnancies, high-risk pregnancies, and records approaching estimated due date.
- Persists the complete Prenatal Record aggregate including all visit data, ultrasound assessments, and labor plans.

### Screening Programs Context

**ScreeningEpisodeRepository**
- Retrieves episodes by episode ID, patient ID, or screening type.
- Supports queries for episodes with abnormal results requiring follow-up, overdue screening episodes, and episodes by result status.
- Persists the complete Screening Episode aggregate including orders, results, and follow-up pathway records.

### Patient Education Context

**EducationSessionRepository**
- Retrieves sessions by session ID, patient ID, or topic.
- Supports queries for sessions with low comprehension scores, sessions linked to specific clinical events, and pending sessions.
- Persists the complete Education Session aggregate including content delivery records and comprehension assessments.

## Repository Design Principles

1. Repositories operate on aggregate roots only. There is no repository for a child entity or value object within an aggregate.
2. Repository interfaces are defined in the domain layer. Implementations belong to the infrastructure layer.
3. Repositories return fully reconstituted aggregates, not partial projections. Read-optimized queries use separate read models (CQRS pattern).
4. Repository methods use domain-meaningful names. Use findActivePregnancies() rather than findByStatusEquals("active").
5. Repositories encapsulate query logic. The domain layer does not construct database queries.
6. Repositories support unit-of-work patterns, ensuring that aggregate modifications are persisted atomically.

## Query Patterns

The gynecology domain requires several common query patterns:

- **By identity**: Retrieve a specific aggregate by its unique identifier.
- **By patient**: Retrieve all aggregates associated with a specific patient across a bounded context.
- **By status**: Retrieve aggregates in a particular workflow state (open, pending, completed).
- **By date range**: Retrieve aggregates created or modified within a time window.
- **By clinical criteria**: Retrieve aggregates matching domain-specific clinical parameters (high-risk pregnancies, abnormal screening results).

## Separation of Read and Write Models

For complex reporting and clinical dashboard requirements, the gynecology domain separates read models from write models following the CQRS pattern. Repositories handle write-side persistence of aggregates. Separate read-model projections handle queries optimized for reporting, list views, and clinical dashboards. This separation ensures that complex query requirements do not compromise aggregate design.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repository implementations.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on repository patterns.
