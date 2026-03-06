# Strategic Design in the Dermatology Domain

Strategic design addresses how to decompose the dermatology domain into manageable bounded contexts, each reflecting a coherent area of clinical practice with its own consistent model and terminology. In dermatology, strategic design is essential because the domain spans diagnostic assessment, surgical intervention, cosmetic practice, laboratory integration, and longitudinal outcome tracking -- each with distinct workflows, stakeholders, and regulatory requirements.

## Decomposition Approach

The dermatology domain is decomposed into six bounded contexts based on natural divisions observed in clinical practice. Dermatology clinics typically organize their operations around patient assessment, lesion-specific management, procedural workflows, cosmetic services, laboratory coordination, and outcome measurement. Each of these areas has its own language, rules, and lifecycle concerns, making them natural candidates for bounded contexts as described by Evans (2003).

The decomposition follows the principle that each bounded context should own a single cohesive model. For example, the Clinical Assessment Context models skin examinations and lesion documentation using morphological descriptors and dermoscopic patterns, while the Procedure Management Context models surgical stages, equipment parameters, and procedural protocols. These models overlap in certain concepts (such as "lesion"), but each context defines the concept differently according to its own needs.

## Subdomain Classification

Strategic design classifies each area of the dermatology domain by its strategic importance to the organization. Following Khononov's (2021) framework, subdomains fall into three categories.

Core subdomains represent the highest-value differentiating capabilities. In dermatology, Clinical Assessment, Lesion Management, and Procedure Management are core because they embody specialized clinical expertise that distinguishes one dermatology practice from another. These areas require deep domain modeling and represent the primary competitive advantage.

Supporting subdomains provide essential functionality that enables core operations. Cosmetic Services and Treatment Outcomes fall into this category. While important, these areas are less uniquely differentiating and may follow more standardized patterns.

Generic subdomains solve common problems with established solutions. Pathology Integration follows well-defined interoperability standards (HL7, FHIR) and can leverage existing laboratory information system integrations rather than requiring custom domain modeling.

## Context Boundaries

Defining clear boundaries between contexts prevents model corruption and reduces coupling. In the dermatology domain, boundaries align with organizational responsibility. The dermatologist performing a skin exam operates within the Clinical Assessment Context. When a biopsy is decided upon, the workflow transitions to the Lesion Management Context, which coordinates with the Pathology Integration Context for specimen handling.

Boundaries are enforced through well-defined interfaces. When a lesion identified in the Clinical Assessment Context requires a procedure, the Clinical Assessment Context publishes a domain event rather than directly manipulating procedure models. This preserves the autonomy of each context and prevents the entanglement of assessment logic with procedural logic.

## Alignment with Clinical Workflow

Strategic design in dermatology aligns bounded contexts with the natural flow of patient care. A typical clinical encounter begins in the Clinical Assessment Context (examination and documentation), may transition to the Lesion Management Context (biopsy decision), proceed to the Procedure Management Context (treatment), interact with the Pathology Integration Context (specimen analysis), and conclude in the Treatment Outcomes Context (follow-up assessment). Each context handles its portion of the workflow independently, communicating through domain events and shared identifiers.

## Strategic Design Decisions

Key strategic decisions in the dermatology domain include the separation of medical and cosmetic workflows into distinct bounded contexts (reflecting different regulatory requirements, consent processes, and billing models), the treatment of pathology as an integration concern rather than a core capability (recognizing that pathology laboratories are typically external organizations), and the elevation of treatment outcomes to its own context (acknowledging the growing importance of evidence-based outcome tracking in dermatology practice).

## Distillation

Following Evans' (2003) concept of domain distillation, the core domain model focuses on what makes dermatology practice unique: the clinical assessment of skin conditions, the management of lesions through their diagnostic and treatment lifecycle, and the execution of specialized procedures. Generic concerns such as appointment scheduling, billing, and patient demographics are excluded from the core domain model and treated as supporting infrastructure.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapters 14-15 on strategic design and distillation.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on strategic design with bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 1-4 on analyzing business domains and subdomain classification.
- Bolognia, Jean L., et al. *Dermatology*. Elsevier, 4th Edition, 2017.
