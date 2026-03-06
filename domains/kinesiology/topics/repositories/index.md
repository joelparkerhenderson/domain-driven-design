# Repositories - Kinesiology Domain

## Overview

Repositories provide an abstraction for storing and retrieving aggregate roots. They present a collection-like interface to the domain layer, hiding the details of data storage and retrieval. The domain model interacts with repositories using domain concepts, not database queries. A repository for MovementAssessment aggregates returns MovementAssessment objects, not rows or documents.

In the kinesiology domain, repositories enable the domain model to remain pure and independent of infrastructure concerns. Whether data is stored in a relational database, a document store, or an in-memory collection, the domain logic remains unchanged. This separation is essential for maintaining the integrity of clinical and scientific models that must be correct regardless of the storage technology.

## Repository Design Principles

Each aggregate root has exactly one repository. Internal entities and value objects within an aggregate are accessed through the aggregate root, never through their own repositories. This enforces the aggregate boundary and ensures consistency invariants are maintained.

Repositories should express their interface using the ubiquitous language. Methods should be named in domain terms, not data access terms. Use findBySubjectId rather than selectWhereSubjectIdEquals. Use save rather than insertOrUpdate.

Repositories should support reconstitution, which is the process of rebuilding a complete aggregate from its stored representation. When a MovementAssessment is retrieved, the repository returns a fully hydrated aggregate with all internal entities and value objects properly constructed and validated.

Repositories should support specification patterns for complex queries. Rather than adding a new method for every query variation, specifications encapsulate query criteria as objects that can be composed and reused.

## Movement Assessment Context Repositories

### MovementAssessmentRepository

This repository manages the persistence and retrieval of MovementAssessment aggregates. Key operations include saving a new or modified assessment, finding an assessment by its unique identifier, finding all assessments for a specific subject, and finding assessments by date range or evaluator.

The repository ensures that when an assessment is retrieved, all internal entities (GaitAnalysis, RangeOfMotionTest, FunctionalMovementScreen) and value objects (JointAngle, MuscleGrade, MovementScore) are fully reconstituted. The domain model receives a complete, valid aggregate ready for use.

Query specifications enable finding assessments with specific characteristics, such as all assessments for a subject where a particular movement deficit was identified, or all assessments performed by a specific evaluator within a date range.

### GaitAnalysisSessionRepository

This repository manages GaitAnalysisSession aggregates, which may exist independently of the broader MovementAssessment aggregate when gait analysis is performed as a standalone evaluation. It supports retrieval by session identifier, by subject, and by analysis conditions (treadmill versus overground, shod versus barefoot).

## Exercise Prescription Context Repositories

### ExerciseProgramRepository

This repository manages ExerciseProgram aggregates. Key operations include saving programs, finding programs by subject, finding active programs, and finding programs by periodization phase. The repository reconstitutes the complete periodization hierarchy (Macrocycle, Mesocycle, Microcycle, TrainingSession) when retrieving a program.

Query specifications support finding programs by training emphasis, modality, or status, enabling analysis of programming patterns across a population.

### ExerciseCatalogRepository

This repository manages ExerciseCatalog aggregates. It supports finding exercises by muscle group target, equipment requirement, difficulty level, or contraindication profile. The catalog repository is typically read-heavy with infrequent writes, making it a candidate for caching strategies.

## Rehabilitation Planning Context Repositories

### RehabilitationPlanRepository

This repository manages RehabilitationPlan aggregates. Key operations include saving plans, finding plans by subject, finding plans by injury type, and finding plans by current rehabilitation phase. The repository reconstitutes the complete phase structure with all milestones and therapeutic exercise sets.

Query specifications support clinical queries such as finding all plans currently in the return-to-play phase, or finding all plans for a specific injury type where a particular milestone was achieved within a specified timeframe.

## Performance Analysis Context Repositories

### PerformanceAssessmentRepository

This repository manages PerformanceAssessment aggregates. It supports finding assessments by subject, by assessment type (force plate, motion capture), by date range, and by specific performance metrics. Given the data-intensive nature of performance analysis, this repository may need to support partial loading strategies where summary data is loaded initially and detailed trial data is loaded on demand.

## Injury Prevention Context Repositories

### InjuryRiskProfileRepository

This repository manages InjuryRiskProfile aggregates. Key operations include finding profiles by subject, finding profiles that exceed specific risk thresholds, and finding profiles with active alerts. The repository must support efficient retrieval of current risk status while also preserving historical risk factor data for trend analysis.

### WorkloadEntryRepository

While individual WorkloadEntry entities exist within the InjuryRiskProfile aggregate, the volume and time-series nature of workload data may justify a specialized repository interface for efficient temporal queries. This repository supports finding workload entries by subject and date range, calculating rolling averages, and identifying workload spikes.

## Research and Education Context Repositories

### EvidenceBaseRepository

This repository manages EvidenceBase aggregates. It supports finding evidence by clinical topic, by level of evidence, by publication date, and by guideline status. The repository enables the Research and Education Context to serve as a knowledge utility for all other contexts.

## Cross-Cutting Repository Concerns

Repository implementations should be transactional at the aggregate level. Saving an aggregate either succeeds completely or fails completely, ensuring the aggregate's consistency invariants are never violated by partial saves.

Repositories should support optimistic concurrency control to prevent lost updates when multiple users or processes modify the same aggregate concurrently. This is particularly important in clinical contexts where multiple practitioners may be working with the same patient's data.

Repository interfaces belong to the domain layer. Repository implementations belong to the infrastructure layer. This separation ensures that the domain model can be tested with in-memory repository implementations and deployed with production database implementations.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12 on repository patterns.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7 on persistence patterns.
