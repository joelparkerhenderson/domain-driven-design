# Hospital Management - Domain-Driven Design

Domain-Driven Design (DDD) for hospital management creates a modular and scalable system by modeling software directly after complex healthcare workflows using a shared "ubiquitous language" between developers and clinicians. It breaks the system into bounded contexts such as EMR, scheduling, and billing to manage complexity.

## Bounded Contexts

- **[Patient Context](topics/patient-context/)** - Registration, demographics, medical history
- **[Appointment/Scheduling Context](topics/appointment-scheduling-context/)** - Doctor schedules, appointments, resource allocation
- **[Clinical/EMR Context](topics/clinical-emr-context/)** - Diagnostics, lab tests, imaging, treatment plans
- **[Billing/Administrative Context](topics/billing-administrative-context/)** - Invoices, insurance claims, payments
- **[Emergency Services Context](topics/emergency-services-context/)** - Triage, urgent care, trauma protocols

## Strategic Design

- **[Strategic Design](topics/strategic-design/)** - Principles for structuring the hospital domain
- **[Bounded Contexts](topics/bounded-contexts/)** - Defining model boundaries
- **[Context Map](topics/context-map/)** - Relationships between bounded contexts
- **[Ubiquitous Language](topics/ubiquitous-language/)** - Shared vocabulary for developers and clinicians
- **[Subdomains](topics/subdomains/)** - Core, supporting, and generic subdomains
- **[Anti-Corruption Layer](topics/anti-corruption-layer/)** - Protecting domain models from external systems

## Tactical Design

- **[Aggregates](topics/aggregates/)** - Clusters of entities and value objects
- **[Entities](topics/entities/)** - Objects with unique identity
- **[Value Objects](topics/value-objects/)** - Immutable objects defined by attributes
- **[Domain Events](topics/domain-events/)** - Significant occurrences in the domain
- **[Repositories](topics/repositories/)** - Persistence abstraction for aggregates
- **[Domain Services](topics/domain-services/)** - Operations spanning multiple aggregates

## Cross-Cutting Concerns

- **[Event-Driven Architecture](topics/event-driven-architecture/)** - Event flows, sagas, choreography vs. orchestration
- **[Integration Patterns](topics/integration-patterns/)** - Shared kernel, published language, open host service
- **[Healthcare Standards](topics/healthcare-standards/)** - HL7, FHIR, ICD-10, CPT, SNOMED CT, LOINC
- **[Regulatory Compliance](topics/regulatory-compliance/)** - HIPAA, consent management, audit trails

## Key Files

- [Ubiquitous Language Glossary](language.md) - Canonical list of domain terms
- [Project Plan](plan.md) - Implementation plan and phases
- [Tasks](tasks.md) - Task tracking checklist
- [All Topics](topics/) - Complete topic listing

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
- HL7 International. "HL7 FHIR R4 Specification." https://hl7.org/fhir/
- U.S. Department of Health and Human Services. "HIPAA Privacy Rule." 45 CFR Part 164.
