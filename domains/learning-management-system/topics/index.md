# Learning Management System DDD - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - Strategic design principles applied to LMS
- [Bounded Contexts](bounded-contexts/) - The six bounded contexts and their boundaries
- [Context Map](context-map/) - Relationships between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared vocabulary between developers and educators
- [Subdomains](subdomains/) - Core, supporting, and generic subdomains
- [Anti-Corruption Layer](anti-corruption-layer/) - Protecting domain models from external systems

## Tactical Design

- [Aggregates](aggregates/) - Clusters of entities and value objects
- [Entities](entities/) - Objects with unique identity
- [Value Objects](value-objects/) - Immutable objects defined by attributes
- [Domain Events](domain-events/) - Significant occurrences in the domain
- [Repositories](repositories/) - Persistence abstraction for aggregate roots
- [Domain Services](domain-services/) - Operations spanning multiple aggregates

## Bounded Contexts

- [Course Catalog Context](course-catalog-context/) - Course definitions, curriculum, learning paths, prerequisites
- [Enrollment Context](enrollment-context/) - Registration, waitlists, enrollment policies
- [Content Delivery Context](content-delivery-context/) - Lessons, modules, multimedia, SCORM, content sequencing
- [Assessment Context](assessment-context/) - Quizzes, exams, assignments, grading, rubrics, feedback
- [Learner Progress Context](learner-progress-context/) - Progress tracking, completion, certificates, transcripts
- [Collaboration Context](collaboration-context/) - Forums, group projects, peer review, messaging

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Event flows, sagas, choreography vs. orchestration
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service
- [LMS Standards](lms-standards/) - SCORM, xAPI, LTI, Caliper, QTI
- [Regulatory Compliance](regulatory-compliance/) - FERPA, accessibility, accreditation
