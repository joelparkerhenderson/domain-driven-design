# Urology Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/index.md) - High-level decomposition of the urology domain into bounded contexts and subdomains
- [Bounded Contexts](bounded-contexts/index.md) - The six logical boundaries that partition the urology domain model
- [Context Map](context-map/index.md) - Relationships and integration points between urology bounded contexts
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared terminology between urologists, developers, and stakeholders
- [Subdomains](subdomains/index.md) - Classification of urology areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries protecting urology contexts from external systems

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of related urology objects treated as consistency units
- [Entities](entities/index.md) - Objects with unique identity that persist across the urological care lifecycle
- [Value Objects](value-objects/index.md) - Immutable objects representing clinical measurements and scores
- [Domain Events](domain-events/index.md) - Significant occurrences that trigger actions across urology contexts
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving urology aggregate roots
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple urology aggregates

## Bounded Context Documentation

- [Clinical Assessment Context](clinical-assessment-context/index.md) - Urological examination, urodynamics, cystoscopy, imaging
- [Surgical Management Context](surgical-management-context/index.md) - Robotic surgery, laparoscopy, endourology, reconstruction
- [Oncologic Urology Context](oncologic-urology-context/index.md) - Prostate, bladder, kidney cancer staging and surveillance
- [Stone Disease Context](stone-disease-context/index.md) - Nephrolithiasis evaluation, ESWL, ureteroscopy, PCNL
- [Incontinence Management Context](incontinence-management-context/index.md) - Stress/urge incontinence, pelvic floor, surgical options
- [Male Reproductive Health Context](male-reproductive-health-context/index.md) - Infertility, vasectomy, erectile dysfunction, testosterone

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between urology bounded contexts through domain events
- [Integration Patterns](integration-patterns/index.md) - Shared kernel, published language, and open host service patterns
- [Urology Standards](urology-standards/index.md) - AUA guidelines, TNM staging, Gleason scoring, and clinical standards
- [Regulatory Compliance](regulatory-compliance/index.md) - HIPAA, FDA regulations, and clinical trial governance
