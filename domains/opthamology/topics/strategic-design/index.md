# Strategic Design in Ophthalmology

## Overview

Strategic design in Domain-Driven Design addresses the high-level decomposition of a
complex system into manageable parts. In the ophthalmology domain, strategic design
determines how clinical, diagnostic, surgical, refractive, glaucoma, and retinal
subsystems relate to one another and where the boundaries between them lie. These
decisions shape the overall architecture and govern how domain knowledge flows between
teams of ophthalmologists, optometrists, technicians, and developers.

## Purpose

The ophthalmology domain is inherently complex. A single patient encounter may involve
visual acuity testing, slit lamp examination, OCT imaging, and a treatment decision
that spans glaucoma management and retinal care. Strategic design provides the tools
to manage this complexity without creating a single monolithic model that attempts to
represent all clinical concepts uniformly. Instead, each bounded context maintains its
own internally consistent model, and strategic patterns govern the integration points.

## Key Strategic Patterns

### Bounded Contexts

Six bounded contexts partition the ophthalmology domain along natural clinical lines.
The Clinical Examination Context owns the model for patient encounters and examination
findings. The Diagnostic Imaging Context owns imaging studies and their interpretation.
The Surgical Management Context owns operative planning and perioperative care. Each
context has autonomy over its internal model and data ownership.

### Context Mapping

The context map documents how bounded contexts interact. For example, the Clinical
Examination Context is upstream to the Diagnostic Imaging Context, because examination
findings drive imaging orders. The Diagnostic Imaging Context is upstream to Surgical
Management, because imaging results inform operative planning. These relationships are
classified using patterns such as customer-supplier, conformist, and anti-corruption
layer depending on the degree of coupling required.

### Subdomain Classification

Not all parts of the ophthalmology domain deliver equal strategic value. Core subdomains
such as Clinical Examination and Surgical Management represent the areas where deep
domain expertise provides competitive advantage and clinical quality. Supporting
subdomains like Diagnostic Imaging and Retinal Care provide essential but more
standardized capabilities. Generic subdomains like Refractive Services follow
well-established protocols that can leverage commodity solutions.

### Anti-Corruption Layers

Ophthalmology systems must integrate with external systems including electronic health
records (EHR), billing platforms, laboratory information systems, and imaging archives
(PACS). Anti-corruption layers translate between the ophthalmology domain model and the
models imposed by these external systems, preventing external concepts from corrupting
the internal domain model.

## Strategic Design Decisions

The following principles guide strategic design in the ophthalmology domain:

- Each bounded context corresponds to a clinical subspecialty or functional area with
  its own vocabulary and workflows.
- Context boundaries align with organizational boundaries where possible, reflecting
  how ophthalmology practices are structured.
- Integration between contexts uses domain events to maintain loose coupling.
- Shared clinical concepts (e.g., patient identity, eye laterality) are handled through
  a published language rather than a shared kernel to avoid tight coupling.
- External system integration always passes through an anti-corruption layer.

## Relationship to Tactical Design

Strategic design sets the stage for tactical design. Once bounded context boundaries
are established, tactical patterns (entities, value objects, aggregates, domain events,
repositories, domain services) are applied within each context to model the internal
business logic. Strategic design answers "what belongs where," while tactical design
answers "how is it modeled."

## Stakeholder Alignment

Strategic design requires ongoing collaboration between domain experts and developers.
In the ophthalmology domain, this means ophthalmologists, optometrists, ophthalmic
technicians, clinical informaticists, and software engineers must jointly define
context boundaries and ubiquitous language. The context map serves as a shared artifact
that all stakeholders can reference to understand system structure and responsibilities.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapters 14-17.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 7-9.
- American Academy of Ophthalmology. *Basic and Clinical Science Course (BCSC)*. Section 1: Update on General Medicine.
