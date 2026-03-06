# Strategic Design: Attention-Deficit Domain

Strategic design addresses how the ADHD management domain is decomposed into manageable bounded contexts, how those contexts relate to one another, and how the domain's ubiquitous language ensures consistent communication among clinicians, educators, therapists, and system developers.

## Purpose

The attention-deficit domain spans multiple disciplines: psychiatry, psychology, education, pharmacology, and behavioral science. Strategic design provides the framework for managing this inherent complexity by identifying clear boundaries between different areas of ADHD management and defining explicit relationships between them.

Without strategic design, an ADHD management system risks becoming a monolithic tangle where clinical assessment logic bleeds into medication management, educational accommodation rules become coupled to treatment planning, and outcomes tracking is scattered across every component.

## Domain Decomposition

The ADHD management domain decomposes into six bounded contexts, each representing a distinct area of expertise and responsibility:

- **Clinical Assessment Context** serves as the diagnostic foundation, owning the patient's assessment history and diagnostic profile based on DSM-5 criteria.
- **Treatment Management Context** coordinates multimodal interventions across behavioral therapy, parent training, coaching, and psychoeducation.
- **Behavioral Monitoring Context** captures longitudinal symptom data, behavior charts, and executive function performance metrics.
- **Educational Accommodation Context** manages the interface between clinical recommendations and school-based supports including IEP and 504 plans.
- **Medication Management Context** handles pharmacological treatment protocols, dosage titration, and side effect monitoring.
- **Outcomes Tracking Context** aggregates treatment effectiveness data across all other contexts to evaluate overall progress.

## Strategic Importance Classification

Subdomains are classified by their strategic value to the ADHD management system:

- **Core Subdomains**: Clinical assessment and treatment management represent the primary competitive differentiators. These areas require the deepest domain expertise and most sophisticated modeling.
- **Supporting Subdomains**: Behavioral monitoring and educational accommodation enable core processes but follow more standardized patterns.
- **Generic Subdomains**: Medication management and outcomes tracking can leverage established pharmaceutical and measurement frameworks.

## Context Relationships

Strategic design defines how bounded contexts interact through patterns such as:

- **Customer-Supplier**: Clinical Assessment supplies diagnostic data to Treatment Management and Medication Management.
- **Conformist**: Educational Accommodation conforms to external school district systems and regulatory frameworks.
- **Anti-Corruption Layer**: Translation boundaries protect clinical terminology from educational system concepts and vice versa.
- **Published Language**: Standardized formats based on DSM-5 codes, rating scale scoring, and clinical practice guidelines.

## Key Principles

**Protect domain model integrity.** Each bounded context maintains its own internally consistent model of the ADHD management domain. A "patient" in the Clinical Assessment Context has different attributes and behaviors than a "student" in the Educational Accommodation Context, even when referring to the same individual.

**Align boundaries with team expertise.** Bounded context boundaries should reflect the organizational structure of ADHD care teams. Clinicians own the assessment context, therapists own treatment management, educators own accommodation planning.

**Embrace controlled duplication.** Sharing a single patient model across all contexts creates tight coupling. Strategic design accepts that each context will maintain its own representation, with explicit translation at context boundaries.

**Design for evolution.** ADHD management practices evolve as new research emerges, diagnostic criteria are updated, and treatment guidelines change. Strategic design ensures that changes within one context do not cascade across the entire system.

## Relationship to DDD Literature

Evans (2003) established strategic design as the mechanism for managing complexity in large domains, emphasizing that the most critical design decisions are about boundaries and relationships between models, not the internal structure of any single model. Vernon (2013) expanded on strategic patterns with practical guidance for context mapping and subdomain classification. Khononov (2021) provides accessible coverage of how strategic design decisions drive the overall architecture of domain-driven systems.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- American Psychiatric Association. (2013). *Diagnostic and Statistical Manual of Mental Disorders* (5th ed.).
- Barkley, R. A. (2015). *Attention-Deficit Hyperactivity Disorder: A Handbook for Diagnosis and Treatment* (4th ed.). Guilford Press.
