# Cardiology Domain - Topics

## Strategic Design Patterns

- [Strategic Design](strategic-design/) - High-level decomposition of the cardiology domain into bounded contexts and subdomains.
- [Bounded Contexts](bounded-contexts/) - Logical boundaries where specific cardiology models and terminologies are consistent.
- [Context Map](context-map/) - Relationships and interactions between the six cardiology bounded contexts.
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology between cardiologists, technologists, and developers.
- [Subdomains](subdomains/) - Classification of cardiology areas by strategic importance (core, supporting, generic).
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting cardiology contexts from external system concepts.

## Tactical Design Patterns

- [Aggregates](aggregates/) - Clusters of related cardiology objects treated as consistency units.
- [Entities](entities/) - Cardiology objects with unique identity that persist over time.
- [Value Objects](value-objects/) - Immutable cardiology measurements, classifications, and scores.
- [Domain Events](domain-events/) - Clinically significant occurrences that trigger cross-context workflows.
- [Repositories](repositories/) - Abstractions for storing and retrieving cardiology aggregate roots.
- [Domain Services](domain-services/) - Stateless operations spanning multiple cardiology aggregates.

## Bounded Context Documentation

- [Clinical Assessment Context](clinical-assessment-context/) - History/physical, risk stratification, cardiac biomarkers, stress testing.
- [Diagnostic Imaging Context](diagnostic-imaging-context/) - Echocardiography, cardiac CT, cardiac MRI, nuclear cardiology.
- [Interventional Procedures Context](interventional-procedures-context/) - Cardiac catheterization, PCI/stenting, TAVR, structural heart.
- [Electrophysiology Context](electrophysiology-context/) - Arrhythmia management, ablation, pacemaker/ICD, EP studies.
- [Heart Failure Management Context](heart-failure-management-context/) - HFrEF/HFpEF classification, GDMT, mechanical circulatory support.
- [Cardiac Rehabilitation Context](cardiac-rehabilitation-context/) - Exercise programs, risk factor modification, lifestyle counseling.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Communication between cardiology bounded contexts through domain events.
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service patterns.
- [Cardiology Standards](cardiology-standards/) - ACC/AHA guidelines, ICD-10, CPT coding, clinical standards.
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, CMS, clinical trial requirements, patient safety regulations.
