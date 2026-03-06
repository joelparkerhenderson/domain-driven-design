# Autism Domain: Topics

## Strategic Design

- [Strategic Design](strategic-design/) - High-level decomposition and architectural vision for the autism management domain
- [Bounded Contexts](bounded-contexts/) - Six logical boundaries with consistent models and terminology
- [Context Map](context-map/) - Relationships and integration patterns between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology across clinical, therapeutic, and educational stakeholders
- [Subdomains](subdomains/) - Classification of domain areas as core, supporting, or generic
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting each context from external system concepts

## Tactical Design

- [Aggregates](aggregates/) - Consistency boundaries grouping related domain objects within each bounded context
- [Entities](entities/) - Objects with unique identity that persist over time as their attributes change
- [Value Objects](value-objects/) - Immutable objects defined by their attributes rather than identity
- [Domain Events](domain-events/) - Significant occurrences within the domain that trigger cross-context actions
- [Repositories](repositories/) - Abstractions for storing and retrieving aggregate roots
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates within a bounded context

## Bounded Contexts

- [Diagnostic Assessment Context](diagnostic-assessment-context/) - Screening, diagnostic evaluation, and multi-disciplinary assessment
- [Behavioral Intervention Context](behavioral-intervention-context/) - ABA therapy, behavior plans, and skill acquisition
- [Sensory Processing Context](sensory-processing-context/) - Sensory profiles, sensory diets, and environmental modifications
- [Communication Support Context](communication-support-context/) - AAC, speech therapy, PECS, and social communication
- [Educational Planning Context](educational-planning-context/) - IEP development, transition planning, and inclusive education
- [Outcomes Tracking Context](outcomes-tracking-context/) - Milestones, treatment effectiveness, and quality of life

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Asynchronous communication patterns between bounded contexts
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, and other collaboration strategies
- [Autism Standards](autism-standards/) - Clinical guidelines and diagnostic standards informing domain model design
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, FERPA, IDEA, ADA, and state autism insurance mandates
