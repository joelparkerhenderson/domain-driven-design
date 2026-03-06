# Psychology Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - High-level decomposition of the psychology domain into bounded contexts and subdomains
- [Bounded Contexts](bounded-contexts/) - Logical boundaries where specific psychology models and terminology are consistent
- [Context Map](context-map/) - Relationships and interactions between the six psychology bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology bridging clinicians, researchers, and system developers
- [Subdomains](subdomains/) - Classification of psychology domain areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting bounded contexts from external system concepts

## Tactical Design

- [Aggregates](aggregates/) - Clusters of related psychology objects treated as consistency units
- [Entities](entities/) - Psychology objects with unique identity persisting over time
- [Value Objects](value-objects/) - Immutable psychology objects defined by their attributes
- [Domain Events](domain-events/) - Clinically significant occurrences triggering cross-context actions
- [Repositories](repositories/) - Abstractions for storing and retrieving psychology aggregate roots
- [Domain Services](domain-services/) - Stateless operations spanning multiple psychology aggregates

## Bounded Context Details

- [Psychological Assessment Context](psychological-assessment-context/) - Psychometric testing, personality inventories, intelligence testing, neuropsychological assessment
- [Therapeutic Intervention Context](therapeutic-intervention-context/) - Evidence-based therapies, treatment protocols, session management
- [Behavioral Analysis Context](behavioral-analysis-context/) - Functional behavior assessment, behavior modification, ABA
- [Cognitive Assessment Context](cognitive-assessment-context/) - Cognitive screening, executive function, memory, attention testing
- [Research Methods Context](research-methods-context/) - Experimental design, statistical analysis, IRB protocols, publications
- [Outcomes Measurement Context](outcomes-measurement-context/) - Treatment effectiveness, standardized measures, PROs, benchmarking

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Communication between bounded contexts through domain events
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service patterns
- [Psychology Standards](psychology-standards/) - Domain-specific standards informing value object design
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, APA ethics, and governance requirements shaping the domain model
