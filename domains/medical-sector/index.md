# Medical - Domain-Driven Design

Domain-Driven Design (DDD) for medical practice creates a modular and scalable system by modeling software directly after clinical workflows, diagnostic processes, pharmacy operations, insurance claims, and telemedicine using a shared "ubiquitous language" between developers and clinicians. It breaks the system into bounded contexts to manage the complexity inherent in modern medical practice.

This domain is distinct from hospital management. Medical focuses on clinical and medical practice concerns -- patient records, clinical decision-making, diagnostics, medications, insurance reimbursement, and virtual care -- rather than hospital operations such as bed management, staffing, and facility logistics.

## Bounded Contexts

- **[Patient Records Context](topics/patient-records-context/)** - EHR, patient demographics, medical history, problem lists
- **[Clinical Workflow Context](topics/clinical-workflow-context/)** - Orders, clinical decision support, care plans, referrals
- **[Diagnostics & Imaging Context](topics/diagnostics-imaging-context/)** - Lab orders, results, radiology, pathology, imaging
- **[Pharmacy & Medication Context](topics/pharmacy-medication-context/)** - Prescriptions, drug interactions, formulary, medication administration
- **[Insurance & Claims Context](topics/insurance-claims-context/)** - Coverage verification, claims submission, adjudication, EOB
- **[Telemedicine Context](topics/telemedicine-context/)** - Virtual visits, remote monitoring, telehealth scheduling

## Strategic Design

- **[Strategic Design](topics/strategic-design/)** - Principles for structuring the medical domain
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
- **[Medical Standards](topics/medical-standards/)** - HL7 FHIR, DICOM, ICD-10, CPT, SNOMED CT, LOINC, NDC, RxNorm
- **[Regulatory Compliance](topics/regulatory-compliance/)** - HIPAA, HITECH, FDA 21 CFR Part 11, GDPR health data

## Key Files

- [Ubiquitous Language Glossary](ubiquitous-language.md) - Canonical list of domain terms
- [Project Plan](plan.md) - Implementation plan and phases
- [Tasks](tasks.md) - Task tracking checklist
- [All Topics](topics/) - Complete topic listing

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
- Benson, Tim & Grieve, Grahame. "Principles of Health Interoperability: FHIR, HL7 and SNOMED CT." Springer, 2021.
- HL7 International. "HL7 FHIR R4 Specification." https://hl7.org/fhir/
- U.S. Department of Health and Human Services. "HIPAA Privacy Rule." 45 CFR Part 164.
