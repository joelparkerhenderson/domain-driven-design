# Mental Health Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - Overview of strategic DDD patterns applied to mental health systems
- [Bounded Contexts](bounded-contexts/) - The six bounded contexts that decompose the mental health domain
- [Context Map](context-map/) - Relationships and interactions between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared vocabulary between clinicians and developers
- [Subdomains](subdomains/) - Classification of domain areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting context integrity

## Tactical Design

- [Aggregates](aggregates/) - Consistency boundaries within each bounded context
- [Entities](entities/) - Identity-bearing objects in the mental health domain
- [Value Objects](value-objects/) - Immutable descriptors of clinical concepts
- [Domain Events](domain-events/) - Significant occurrences that trigger cross-context workflows
- [Repositories](repositories/) - Abstractions for persisting and retrieving aggregate roots
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates

## Bounded Context Details

- [Clinical Assessment Context](clinical-assessment-context/) - Diagnostic interviews, standardized assessments, DSM-5 criteria
- [Treatment Planning Context](treatment-planning-context/) - Individualized plans, goal setting, modality selection
- [Therapy Management Context](therapy-management-context/) - Session scheduling, progress notes, therapeutic modalities
- [Crisis Intervention Context](crisis-intervention-context/) - Risk assessment, safety planning, emergency protocols
- [Medication Management Context](medication-management-context/) - Psychotropic prescribing, titration, drug interactions
- [Outcomes Measurement Context](outcomes-measurement-context/) - Symptom tracking, functional assessment, patient-reported outcomes

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Asynchronous communication between bounded contexts
- [Integration Patterns](integration-patterns/) - Strategies for bounded context collaboration
- [Mental Health Standards](mental-health-standards/) - DSM-5, ICD-11, LOINC, HL7 FHIR, and other clinical standards
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, 42 CFR Part 2, and mental health parity regulations
