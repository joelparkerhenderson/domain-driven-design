# Medical DDD - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - Principles for decomposing the medical domain
- [Bounded Contexts](bounded-contexts/) - Logical boundaries for domain models
- [Context Map](context-map/) - Relationships and interactions between contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared vocabulary between developers and clinicians
- [Subdomains](subdomains/) - Core, supporting, and generic classification
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries for external systems

## Tactical Design

- [Aggregates](aggregates/) - Clusters of related objects maintaining consistency
- [Entities](entities/) - Objects with unique identity persisting over time
- [Value Objects](value-objects/) - Immutable objects defined by attributes
- [Domain Events](domain-events/) - Significant occurrences triggering actions
- [Repositories](repositories/) - Abstractions for storing and retrieving aggregates
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates

## Bounded Contexts

- [Patient Records Context](patient-records-context/) - EHR, demographics, medical history, problem lists
- [Clinical Workflow Context](clinical-workflow-context/) - Orders, clinical decision support, care plans, referrals
- [Diagnostics & Imaging Context](diagnostics-imaging-context/) - Lab orders, results, radiology, pathology, imaging
- [Pharmacy & Medication Context](pharmacy-medication-context/) - Prescriptions, drug interactions, formulary, medication administration
- [Insurance & Claims Context](insurance-claims-context/) - Coverage verification, claims submission, adjudication, EOB
- [Telemedicine Context](telemedicine-context/) - Virtual visits, remote monitoring, telehealth scheduling

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Event flows, sagas, choreography vs. orchestration
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service
- [Medical Standards](medical-standards/) - HL7 FHIR, DICOM, ICD-10, CPT, SNOMED CT, LOINC, NDC, RxNorm
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, HITECH, FDA 21 CFR Part 11, GDPR health data
