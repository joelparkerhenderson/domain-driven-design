# Kinesiology Domain - Topics Index

## Overview

This index organizes the kinesiology domain topics into four categories: strategic design patterns that decompose the system into bounded contexts, tactical design patterns that structure business logic within those contexts, bounded context details that document each context's internal model, and cross-cutting concerns that span the entire domain.

## Strategic Design

- [Strategic Design](strategic-design/index.md) - High-level decomposition of the kinesiology domain into bounded contexts and subdomains
- [Bounded Contexts](bounded-contexts/index.md) - The six logical boundaries within which specific kinesiology models and terminology remain consistent
- [Context Map](context-map/index.md) - Visual and textual representation of relationships and interactions between bounded contexts
- [Ubiquitous Language](ubiquitous-language/index.md) - The shared vocabulary used by kinesiologists, clinicians, coaches, researchers, and developers
- [Subdomains](subdomains/index.md) - Classification of kinesiology domain areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries that protect bounded contexts from external system concepts

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of related objects treated as single units of consistency
- [Entities](entities/index.md) - Objects with unique identity that persist over time as attributes change
- [Value Objects](value-objects/index.md) - Immutable objects defined entirely by their attributes
- [Domain Events](domain-events/index.md) - Records of significant occurrences that trigger actions across contexts
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving aggregate roots
- [Domain Services](domain-services/index.md) - Stateless operations that coordinate across multiple aggregates

## Bounded Context Details

- [Movement Assessment Context](movement-assessment-context/index.md) - Gait analysis, biomechanical evaluation, range of motion, functional testing
- [Exercise Prescription Context](exercise-prescription-context/index.md) - Program design, periodization, progressive overload, modality selection
- [Rehabilitation Planning Context](rehabilitation-planning-context/index.md) - Post-injury protocols, return-to-play criteria, therapeutic exercise
- [Performance Analysis Context](performance-analysis-context/index.md) - Force plate analysis, motion capture, sport-specific metrics
- [Injury Prevention Context](injury-prevention-context/index.md) - Screening protocols, movement correction, load management, prehabilitation
- [Research & Education Context](research-education-context/index.md) - Evidence-based practice, curriculum development, continuing education

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between bounded contexts through domain events
- [Integration Patterns](integration-patterns/index.md) - Shared kernel, published language, open host service, customer-supplier
- [Kinesiology Standards](kinesiology-standards/index.md) - Professional standards from ACSM, NSCA, NATA, and related bodies
- [Regulatory Compliance](regulatory-compliance/index.md) - Legal, ethical, and governance requirements shaping domain model design
