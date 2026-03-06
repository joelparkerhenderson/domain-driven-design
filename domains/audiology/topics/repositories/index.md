# Repositories in the Audiology Domain

## Overview

Repositories are abstractions that provide the illusion of an in-memory collection of aggregate roots. They encapsulate the logic required to store, retrieve, and search for aggregates while keeping the domain model free from infrastructure concerns. In the audiology domain, repositories serve as the boundary between the domain model and the persistence mechanism, ensuring that clinical data is stored and retrieved without exposing database-specific details to domain logic.

## Repository Design Principles

Repositories in the audiology domain follow several design principles. Each repository manages exactly one aggregate type. Repositories provide collection-like interfaces (add, remove, find) rather than database-specific operations (insert, update, query). Repositories return fully reconstituted aggregates with all their constituent entities and value objects intact. Repositories enforce aggregate invariants during reconstitution, ensuring that persisted data still satisfies all business rules when loaded back into memory.

## Hearing Assessment Context Repositories

### AudiometricEvaluationRepository

The AudiometricEvaluationRepository manages the persistence and retrieval of AudiometricEvaluation aggregates. It supports retrieval by evaluation identifier, by patient identifier (returning all evaluations for a patient in chronological order), and by date range. This repository is critical for clinical workflows that require comparison of current results to previous evaluations, such as detecting threshold changes over time.

The repository ensures that when an evaluation is retrieved, all associated threshold measurements, speech recognition results, and the computed audiogram are fully reconstituted. The repository validates that the reconstituted aggregate satisfies all invariants, guarding against data corruption.

### SpeechRecognitionEvaluationRepository

The SpeechRecognitionEvaluationRepository manages speech recognition evaluation aggregates. It supports retrieval by evaluation identifier and by patient identifier. This repository enables clinicians to track speech recognition performance longitudinally and correlate speech scores with audiometric thresholds.

## Device Management Context Repositories

### DeviceFittingRepository

The DeviceFittingRepository manages DeviceFitting aggregates. It supports retrieval by fitting identifier, by patient identifier, and by device identifier. This repository enables tracking the complete fitting history for a patient across multiple devices and the complete fitting history for a specific device across adjustments.

The repository handles the complexity of fitting records that reference prescriptive targets, device configurations, and verification measurements. All related value objects are reconstituted as part of the aggregate.

### DeviceRepository

The DeviceRepository manages Device aggregates. It supports retrieval by device identifier (serial number), by patient identifier (current devices assigned to a patient), and by status (active, in repair, retired). This repository tracks device lifecycle from acquisition through retirement.

## Rehabilitation Context Repositories

### RehabilitationPlanRepository

The RehabilitationPlanRepository manages RehabilitationPlan aggregates. It supports retrieval by plan identifier, by patient identifier, and by status (active, completed, discontinued). The repository reconstitutes plans with all associated goals, interventions, and outcome measurements.

This repository supports the longitudinal view of rehabilitation, allowing clinicians to review past plans and their outcomes when designing new intervention approaches for a patient.

## Vestibular Services Context Repositories

### VestibularEvaluationRepository

The VestibularEvaluationRepository manages VestibularEvaluation aggregates. It supports retrieval by evaluation identifier and by patient identifier. The repository reconstitutes evaluations with all associated test results (VNG, caloric, positional) and diagnostic conclusions.

### VestibularRehabilitationProgramRepository

The VestibularRehabilitationProgramRepository manages vestibular rehabilitation program aggregates. It supports retrieval by program identifier and by patient identifier, enabling tracking of exercise prescriptions and functional improvement over the rehabilitation course.

## Pediatric Audiology Context Repositories

### PediatricPatientRepository

The PediatricPatientRepository manages PediatricPatient aggregates. It supports retrieval by patient identifier, by screening status (passed, referred, pending follow-up), and by developmental monitoring status. This repository is essential for tracking compliance with Early Hearing Detection and Intervention (EHDI) timelines.

### ScreeningEventRepository

The ScreeningEventRepository manages ScreeningEvent aggregates. It supports retrieval by screening identifier, by patient identifier, and by result (pass, refer). This repository enables population-level screening program management and individual patient follow-up tracking.

## Clinical Documentation Context Repositories

### ClinicalReportRepository

The ClinicalReportRepository manages ClinicalReport aggregates. It supports retrieval by report identifier, by patient identifier, by report type (audiometric evaluation, fitting report, vestibular evaluation), and by status (draft, finalized, distributed). This repository maintains the complete versioning history of each report.

### ReferralRepository

The ReferralRepository manages Referral aggregates. It supports retrieval by referral identifier, by patient identifier, by referring provider, by receiving provider, and by status (created, sent, acknowledged, fulfilled, closed). This repository enables referral tracking and follow-up compliance monitoring.

## Query Patterns

Repositories in the audiology domain support domain-specific query patterns that reflect clinical workflows. Finding all evaluations for a patient supports longitudinal review. Finding devices by status supports inventory management. Finding referrals by status supports care coordination. These query methods are defined in terms of domain concepts, not database query languages, keeping the domain model infrastructure-agnostic.

## Separation from CQRS Read Models

Repositories serve the command side of the domain model, retrieving aggregates for modification and persisting changes. Read-optimized queries for reporting, dashboards, and clinical summaries may be served by separate read models following CQRS principles. The repository remains focused on aggregate integrity rather than reporting convenience.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 6 on repositories.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 12 on repositories.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 8 on repository patterns.
