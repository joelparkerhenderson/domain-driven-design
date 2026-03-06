# Anti-Corruption Layer: Autism Domain

The anti-corruption layer (ACL) is a translation boundary that protects a bounded context from external system concepts, preventing foreign models from corrupting the internal domain model.

## Purpose

In the autism domain, multiple professional disciplines, external healthcare systems, insurance platforms, and educational databases interact with internal bounded contexts. Each external system carries its own model, terminology, and assumptions. Without an anti-corruption layer, these foreign concepts would leak into bounded contexts, creating model inconsistencies and coupling that undermines the ubiquitous language. The ACL translates between external and internal representations, keeping each bounded context's model clean and consistent.

## Key Anti-Corruption Layers

### Clinical-to-Educational Translation Layer

This ACL sits between the Diagnostic Assessment Context and the Educational Planning Context. Clinical diagnostic terminology differs significantly from educational eligibility language:

- Clinical DSM-5 severity levels (Level 1, Level 2, Level 3) are translated into educational support needs categories used in IEP development.
- Clinical diagnostic reports using medical terminology are translated into educationally relevant summaries that inform eligibility determination.
- Clinical assessment scores (ADOS-2 comparison scores, cognitive testing standard scores) are translated into functional descriptions meaningful to educational teams.

This translation prevents clinical model concepts from being forced into the educational planning model, where they would conflict with IDEA-mandated terminology and processes.

### ABA-to-Educational Translation Layer

This ACL sits between the Behavioral Intervention Context and the Educational Planning Context. ABA-specific terminology requires translation for educational settings:

- ABA technical terms (discriminative stimulus, establishing operation, response cost) are translated into classroom-accessible behavior support language.
- ABA session data formats (trial-by-trial data, interval recording) are translated into progress summaries suitable for IEP progress reporting.
- Behavior function categories from functional behavior assessments are translated into educational behavior support plan language.

### External Healthcare System Integration Layer

This ACL sits at the boundary between the autism domain and external healthcare information systems:

- External EHR (Electronic Health Record) data models, which use generic patient and encounter structures, are translated into the autism domain's specialized Individual, Assessment Session, and Intervention Episode models.
- Insurance claim codes (CPT codes for ABA therapy, speech therapy, occupational therapy) are translated from external billing system formats into domain-relevant service delivery concepts.
- External referral system data is translated into the Diagnostic Assessment Context's intake and screening models.

### External Reporting System Integration Layer

This ACL sits between the Outcomes Tracking Context and external reporting requirements:

- Internal outcome measures are translated into formats required by state autism registries and reporting mandates.
- Internal progress data is translated into insurance-required treatment authorization documentation.
- Internal quality metrics are translated into accreditation body reporting formats (BHCOE, CASP).

## Implementation Principles

- Isolation: The ACL is the only point of contact between the internal model and external systems. No external concepts bypass the ACL.
- Bidirectional Translation: The ACL translates inbound external data into internal model concepts and outbound internal data into external system formats.
- Model Preservation: The ACL never modifies the internal domain model to accommodate external system requirements. External formats are adapted to fit the internal model.
- Failure Handling: When external data cannot be cleanly translated, the ACL raises domain-specific exceptions rather than passing through corrupt or incomplete data.

## Design Considerations

Anti-corruption layers add complexity. They should be implemented only where the risk of model corruption justifies the additional translation logic. In the autism domain, ACLs are essential at the clinical-educational boundary and at external system integration points, where the conceptual distance between models is greatest.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on anti-corruption layers.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3 on context mapping and ACL patterns.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 7 on protecting bounded context boundaries.
