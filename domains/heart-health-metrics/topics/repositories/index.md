# Repositories

Repositories in the heart health metrics domain provide abstractions for storing and retrieving aggregate roots, decoupling the cardiac domain model from persistence infrastructure. They offer collection-like interfaces that allow the domain layer to work with cardiac data aggregates without depending on specific storage technologies.

## Overview

Eric Evans defines a repository as a mechanism for encapsulating storage, retrieval, and search behavior for a collection of objects, emulating an in-memory collection. In the heart health metrics domain, repositories abstract the persistence of cardiac data aggregates such as Patient Cardiac Profiles, ECG Recording Sessions, Blood Pressure Series, Risk Assessment Reports, and Alert Subscriptions.

Repository design in cardiac health monitoring must accommodate the unique characteristics of medical data: high-volume time-series measurements, strict audit trail requirements, regulatory retention policies, and the need for both real-time access and historical analysis. The repository interface hides these complexities from the domain model.

## Key Concepts

**Patient Cardiac Profile Repository.** Provides methods to find, store, and update Patient Cardiac Profile aggregates. Supports lookup by patient identifier, medical record number, or demographic criteria. The interface enables retrieval of a patient's complete cardiac health context without exposing the underlying storage mechanism.

**ECG Recording Repository.** Manages persistence of ECG Recording Session aggregates. Supports retrieval by session identifier, patient identifier, date range, or clinical finding. The interface accommodates the large binary payload of ECG waveform data while keeping the domain model free of storage format concerns.

**Blood Pressure Series Repository.** Stores and retrieves Blood Pressure Series aggregates. Supports queries by monitoring protocol type, date range, and clinical status (complete, in-progress, abandoned). Enables retrieval of historical series for trend analysis without exposing time-series database internals.

**Risk Assessment Repository.** Manages persistence of Risk Assessment Report aggregates. Supports retrieval by patient identifier, scoring model, date range, and risk category. Enables longitudinal tracking of how a patient's cardiovascular risk has evolved over time.

**Alert Subscription Repository.** Stores active Alert Subscription aggregates. Supports queries for all active subscriptions matching a given metric type or patient, enabling the Monitoring and Alerts Context to efficiently evaluate incoming measurements against relevant alert rules.

**Repository Design Principles.** Repositories in the heart health metrics domain follow several principles: they operate only on aggregate roots (not internal entities), they provide domain-oriented query methods (not generic SQL), they enforce the aggregate's invariants during reconstitution, and they support unit-of-work patterns for transactional consistency.

## Domain Examples

When the Cardiac Analysis Context needs to retrieve all ECG recordings for a patient from the past 30 days, it calls the ECG Recording Repository with the patient identifier and date range. The repository returns fully reconstituted ECG Recording Session aggregates, complete with lead data, quality assessments, and any clinical annotations. The domain code is unaware whether the data was stored in a document database, a time-series database, or a distributed file system.

When the Monitoring and Alerts Context receives a heart rate reading, it queries the Alert Subscription Repository for all active subscriptions monitoring heart rate for that patient. The repository returns the relevant Alert Subscription aggregates, which the monitoring logic evaluates against the incoming reading.

## Relationships to Other Topics

- [Aggregates](../aggregates/index.md) - Repositories persist and retrieve aggregate roots.
- [Entities](../entities/index.md) - Repositories use entity identity for lookup operations.
- [Value Objects](../value-objects/index.md) - Repositories reconstitute aggregates including their value objects.
- [Domain Services](../domain-services/index.md) - Domain services use repositories to access aggregates.
- [Bounded Contexts](../bounded-contexts/index.md) - Each bounded context has its own repositories.
- [Regulatory Compliance](../regulatory-compliance/index.md) - Repositories must support audit and retention requirements.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 10 on repository patterns.
- Fowler, Martin. *Patterns of Enterprise Application Architecture*. Addison-Wesley, 2002. Repository and Unit of Work patterns.
- Jensen, P.B. et al. "Mining Electronic Health Records: Towards Better Research Applications and Clinical Care." *Nature Reviews Genetics*, 13(6), 395-405, 2012.
