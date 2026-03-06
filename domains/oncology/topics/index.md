# Oncology Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/index.md) - Overview of strategic DDD patterns applied to oncology systems
- [Bounded Contexts](bounded-contexts/index.md) - Logical boundaries defining consistent models within oncology care
- [Context Map](context-map/index.md) - Relationships and interactions between oncology bounded contexts
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared terminology among oncology domain experts and developers
- [Subdomains](subdomains/index.md) - Classification of oncology areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries protecting oncology contexts from external systems

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of related oncology objects maintaining consistency
- [Entities](entities/index.md) - Objects with unique identity persisting through oncology workflows
- [Value Objects](value-objects/index.md) - Immutable objects representing oncology domain concepts
- [Domain Events](domain-events/index.md) - Significant occurrences triggering actions across oncology contexts
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving oncology aggregate roots
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple oncology aggregates

## Bounded Context Details

- [Diagnosis & Staging Context](diagnosis-staging-context/index.md) - Cancer screening, pathology, TNM staging, molecular profiling
- [Treatment Planning Context](treatment-planning-context/index.md) - Tumor board, multidisciplinary planning, clinical trials, NCCN guidelines
- [Chemotherapy Management Context](chemotherapy-management-context/index.md) - Regimen selection, dose calculation, infusion scheduling, toxicity monitoring
- [Radiation Therapy Context](radiation-therapy-context/index.md) - Simulation, treatment planning, dose delivery, IGRT, brachytherapy
- [Surgical Oncology Context](surgical-oncology-context/index.md) - Resection planning, margin assessment, sentinel node biopsy, reconstruction
- [Survivorship Care Context](survivorship-care-context/index.md) - Follow-up protocols, late effects, psychosocial support, care plans

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between oncology bounded contexts through domain events
- [Integration Patterns](integration-patterns/index.md) - Patterns for connecting oncology contexts with external systems
- [Oncology Standards](oncology-standards/index.md) - Clinical standards informing domain model design (NCCN, ASCO, CTCAE, TNM)
- [Regulatory Compliance](regulatory-compliance/index.md) - Legal and governance requirements shaping the oncology domain model
