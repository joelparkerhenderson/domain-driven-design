# Gynecology Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/index.md) - overview of strategic DDD patterns applied to the gynecology domain
- [Bounded Contexts](bounded-contexts/index.md) - definition and boundaries of each context in the gynecology system
- [Context Map](context-map/index.md) - relationships, dependencies, and integration between bounded contexts
- [Ubiquitous Language](ubiquitous-language/index.md) - shared terminology used by clinicians, developers, and stakeholders
- [Subdomains](subdomains/index.md) - classification into core, supporting, and generic subdomains
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - translation boundaries that protect context models from external concepts

## Tactical Design

- [Aggregates](aggregates/index.md) - clusters of related objects treated as consistency units
- [Entities](entities/index.md) - objects with unique identity that persist over time
- [Value Objects](value-objects/index.md) - immutable objects defined by their attributes
- [Domain Events](domain-events/index.md) - significant occurrences that trigger actions across contexts
- [Repositories](repositories/index.md) - abstractions for storing and retrieving aggregate roots
- [Domain Services](domain-services/index.md) - stateless operations that span multiple aggregates

## Bounded Context Details

- [Clinical Assessment Context](clinical-assessment-context/index.md) - examination, symptom evaluation, diagnostic workup
- [Reproductive Health Context](reproductive-health-context/index.md) - fertility, contraception, menstrual disorder management
- [Surgical Services Context](surgical-services-context/index.md) - minimally invasive surgery, hysterectomy, pelvic floor repair
- [Prenatal Care Context](prenatal-care-context/index.md) - pregnancy monitoring, ultrasound tracking, high-risk management
- [Screening Programs Context](screening-programs-context/index.md) - cervical, breast, and STI screening, cancer detection
- [Patient Education Context](patient-education-context/index.md) - health literacy, shared decision-making, wellness programs

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - asynchronous communication between bounded contexts
- [Integration Patterns](integration-patterns/index.md) - shared kernel, published language, open host service
- [Gynecology Standards](gynecology-standards/index.md) - clinical standards that inform domain model design
- [Regulatory Compliance](regulatory-compliance/index.md) - legal and governance requirements shaping the domain
