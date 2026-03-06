# Hospital Management DDD - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - Strategic design principles applied to hospital management
- [Bounded Contexts](bounded-contexts/) - The five bounded contexts and their boundaries
- [Context Map](context-map/) - Relationships between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared vocabulary between developers and clinicians
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

- [Patient Context](patient-context/) - Registration, demographics, medical history
- [Appointment/Scheduling Context](appointment-scheduling-context/) - Doctor schedules, appointments, resource allocation
- [Clinical/EMR Context](clinical-emr-context/) - Diagnostics, lab tests, imaging, treatment plans
- [Billing/Administrative Context](billing-administrative-context/) - Invoices, insurance claims, payments
- [Emergency Services Context](emergency-services-context/) - Triage, urgent care, trauma protocols

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Event flows, sagas, choreography vs. orchestration
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service
- [Healthcare Standards](healthcare-standards/) - HL7, FHIR, ICD-10, CPT, SNOMED CT, LOINC
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, consent management, audit trails
