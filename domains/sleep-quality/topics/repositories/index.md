# Repositories in the Sleep Quality Domain

## Overview

Repositories provide abstractions for storing and retrieving aggregate roots, allowing the domain model to remain independent of persistence infrastructure. A repository mediates between the domain layer and the data mapping layer, acting like an in-memory collection of aggregate roots. In the sleep quality domain, repositories enable the clinical and behavioral domain logic to operate without knowledge of how sleep data is stored, whether in relational databases, document stores, or time-series databases.

## Sleep Assessment Repositories

### SleepAssessmentRepository

The SleepAssessmentRepository provides access to SleepAssessment aggregate roots. It supports retrieval by assessment ID, by patient ID (returning all assessments for a patient), and by patient ID with date range filtering. It supports adding new assessments and updating existing ones. The repository ensures that when an assessment is retrieved, all contained instrument results, actigraphy summaries, and polysomnography findings are loaded as part of the aggregate.

### SleepDiaryRepository

The SleepDiaryRepository provides access to SleepDiary aggregate roots. It supports retrieval by diary ID and by patient ID. Because sleep diaries can grow large over months of daily entries, the repository supports loading with a date range parameter to retrieve a subset of entries for operational use, while still maintaining the aggregate's invariants over the loaded range. It supports adding entries and updating the diary's computed statistics.

## Sleep Disorders Repositories

### DisorderDiagnosisRepository

The DisorderDiagnosisRepository provides access to DisorderDiagnosis aggregate roots. It supports retrieval by diagnosis ID, by patient ID (returning all diagnoses), and by patient ID filtered by disorder type or clinical status (active, resolved). It supports adding new diagnoses and updating diagnosis status. The repository enforces that duplicate active diagnoses for the same disorder type and patient cannot be persisted.

## Behavioral Interventions Repositories

### InterventionProtocolRepository

The InterventionProtocolRepository provides access to InterventionProtocol aggregate roots. It supports retrieval by protocol ID, by patient ID (returning all protocols), and by patient ID filtered by intervention type or status (active, completed, discontinued). It supports adding new protocols and updating protocol state. The repository loads the complete protocol with all session entities and component value objects.

## Device Integration Repositories

### DeviceFeedRepository

The DeviceFeedRepository provides access to DeviceFeed aggregate roots. It supports retrieval by feed ID, by patient ID (returning all device feeds for a patient), and by device type. It supports adding new feeds, updating feed configuration, and querying active feeds for data ingestion scheduling. The repository handles the potentially large volume of data ingestion records by supporting pagination and date-range filtering.

## Outcomes Tracking Repositories

### OutcomesRecordRepository

The OutcomesRecordRepository provides access to OutcomesRecord aggregate roots. It supports retrieval by record ID and by patient ID. Because outcomes records grow continuously with each new measurement, the repository supports loading with configurable temporal windows (last 30 days, last 90 days, full history) to balance performance with completeness. Trend computation may be performed at the repository level for large datasets.

## Sleep Environment Repositories

### EnvironmentAssessmentRepository

The EnvironmentAssessmentRepository provides access to EnvironmentAssessment aggregate roots. It supports retrieval by assessment ID, by patient ID, and by location identifier. It supports adding new assessments and updating recommendation implementation status.

## Repository Design Principles

Repositories in the sleep quality domain adhere to several design principles. Each repository manages exactly one aggregate type. Repositories expose a collection-oriented interface (add, find, update) rather than a data-access-oriented interface (insert, select, update SQL). The repository interface is defined in the domain layer, while the implementation resides in the infrastructure layer. This separation ensures that the domain model is not coupled to any specific storage technology.

## Query Optimization

Sleep quality data has temporal characteristics that influence repository design. Sleep diary entries, device data, and outcome measurements are time-series data that benefits from date-range indexing. Assessment and diagnosis lookups are primarily by patient ID. The repository abstraction allows the infrastructure layer to apply appropriate indexing and partitioning strategies without affecting the domain model.

## Specification Pattern

For complex queries that go beyond simple ID or date-range lookups, repositories in the sleep quality domain support the specification pattern. For example, finding all patients with active insomnia diagnoses whose sleep efficiency has been below 75 percent for the past two weeks requires combining criteria from multiple value objects. Specifications encapsulate these query predicates as domain objects that can be composed and reused.

## Unit of Work

Repositories coordinate with a unit of work pattern to manage transactional boundaries. Within a single aggregate, all changes are committed atomically. Across aggregates, domain events provide eventual consistency. The unit of work ensures that aggregate persistence and event publication are coordinated, preventing scenarios where an aggregate is persisted but its events are lost, or vice versa.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 6 on repositories.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 12 on repositories.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 10 on persistence patterns.
