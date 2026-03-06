# Oncology Domain - Plan

## Vision

Model oncology care workflows using Domain-Driven Design to create a comprehensive, modular system that supports the full cancer care continuum from diagnosis through survivorship.

## Goals

1. Establish ubiquitous language shared among oncologists, pathologists, radiologists, nurses, pharmacists, and developers.
2. Define bounded contexts that reflect the natural organizational boundaries of oncology care delivery.
3. Model aggregates, entities, and value objects that capture the complexity of cancer diagnosis, treatment, and follow-up.
4. Design domain events that enable communication across bounded contexts while preserving autonomy.
5. Document integration patterns for interoperability with laboratory systems, imaging systems, pharmacy systems, and tumor registries.
6. Address regulatory compliance including HIPAA, FDA regulations, NCI reporting, and clinical trial protocols.

## Scope

### In Scope

- Cancer screening and diagnostic workflows
- Pathology and molecular profiling
- TNM staging and risk stratification
- Tumor board and multidisciplinary treatment planning
- Chemotherapy regimen management and toxicity monitoring
- Radiation therapy planning and delivery
- Surgical oncology planning and assessment
- Survivorship care planning and follow-up
- Clinical trial eligibility and enrollment
- Integration with oncology standards (NCCN, ASCO, CTCAE)

### Out of Scope

- General hospital administration unrelated to oncology
- Billing and insurance processing (handled by separate domain)
- User interface and front-end design
- Infrastructure and deployment architecture
- Image rendering and DICOM viewing

## Milestones

1. Define ubiquitous language and bounded contexts
2. Model strategic design patterns (subdomains, context map, anti-corruption layers)
3. Model tactical design patterns (aggregates, entities, value objects, domain events)
4. Document each bounded context in detail
5. Document cross-cutting concerns (events, integration, standards, compliance)
6. Review, harmonize, and audit all documentation

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
