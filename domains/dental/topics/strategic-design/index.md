# Strategic Design - Dental Domain

## Overview

Strategic design in Domain-Driven Design addresses how to decompose a large dental practice system into manageable bounded contexts that align with the natural divisions of dental care delivery. The dental domain presents a system where clinical procedures, treatment planning, specialty care, and administrative operations intersect, requiring careful boundary definition to manage complexity.

Strategic design for dental systems focuses on identifying where different models of the same concept coexist. A "patient" in the Clinical Examination Context is primarily a clinical record holder, while in the Practice Management Context the same person is an appointment participant and insurance subscriber. These model differences define the natural seams along which bounded contexts should be drawn.

## Strategic Design Principles in Dentistry

### Alignment with Clinical Workflow

Dental practices operate through a predictable workflow: examination leads to diagnosis, diagnosis informs treatment planning, treatment planning drives procedure execution, and procedure outcomes feed back into the clinical record. Strategic design captures this flow by defining bounded contexts that own specific stages of the workflow while communicating through well-defined domain events.

### Separation of Clinical and Administrative Concerns

Clinical decision-making follows dental science and professional judgment. Administrative operations follow business rules, insurance contract terms, and regulatory requirements. Strategic design separates these concerns so that clinical models remain focused on patient care while administrative models handle scheduling, billing, and compliance without contaminating clinical logic.

### Specialty Isolation

Orthodontic treatment and periodontal care operate on fundamentally different timescales and clinical models compared to general restorative dentistry. Orthodontic cases span months or years with incremental progress tracking. Periodontal care follows a disease management model with recurring maintenance cycles. Strategic design isolates these specialties into their own bounded contexts with appropriate models for their distinct clinical patterns.

## Context Identification Process

The dental domain decomposes into six bounded contexts based on analysis of:

1. Language boundaries where the same term carries different meaning across clinical areas.
2. Data ownership patterns identifying which processes are authoritative for specific information.
3. Temporal patterns distinguishing single-visit procedures from multi-month treatment courses.
4. Regulatory boundaries where different compliance rules apply to different activities.
5. Expertise boundaries separating general dentistry from periodontic and orthodontic specialties.

## Subdomain Classification

The dental domain classifies its areas by strategic importance:

- Core subdomains: Clinical Examination and Treatment Planning, where diagnostic accuracy and treatment sequencing create the most practice value.
- Supporting subdomains: Restorative Services, Orthodontic Management, and Periodontal Care, which implement established clinical protocols.
- Generic subdomains: Practice Management functions like scheduling and insurance processing, which are common across healthcare practices.

## Context Relationships

The six bounded contexts interact through several relationship patterns. The Clinical Examination Context serves as an upstream supplier of diagnostic data to the Treatment Planning Context. The Treatment Planning Context acts as an upstream coordinator distributing approved treatment items to the Restorative Services, Orthodontic Management, and Periodontal Care contexts. The Practice Management Context operates as a supporting context providing scheduling and billing services to all clinical contexts.

## Anti-Corruption Layers

Anti-corruption layers protect clinical models from the influence of external systems such as insurance clearinghouses, electronic health record platforms, and dental supply ordering systems. These translation boundaries ensure that the internal domain model remains consistent with dental clinical concepts rather than being distorted by the data formats and terminology of external integrations.

## Strategic Design Outcomes

Effective strategic design for the dental domain produces:

- Clear ownership of dental data and clinical decisions within each bounded context.
- Reduced coupling between clinical care delivery and administrative operations.
- Independent evolution of specialty clinical models without impacting general dentistry workflows.
- Well-defined integration points for external systems through anti-corruption layers.
- A shared ubiquitous language that dental professionals and software developers use consistently.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapters 14-16 on strategic design.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Part II on strategic design with bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 7-9 on modeling boundaries.
- American Dental Association. Standards Committee on Dental Informatics. ADA Technical Reports.
