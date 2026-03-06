# Strategic Design in Gastroenterology

## Overview

Strategic design in Domain-Driven Design addresses the high-level decomposition of a
complex domain into bounded contexts, each with well-defined boundaries and clear
ownership of specific clinical concepts. In gastroenterology, strategic design determines
how the broad clinical domain is partitioned into contexts that reflect the natural
organization of gastroenterology practice.

## Purpose

Gastroenterology spans multiple clinical subspecialties: diagnostic evaluation, endoscopy,
inflammatory disease management, hepatology, motility assessment, and nutrition. Without
strategic design, these areas risk becoming a tangled monolith where terminology conflicts,
model ambiguity, and tightly coupled logic make the system difficult to evolve.

Strategic design prevents this by establishing clear boundaries. Each bounded context owns
its models, its language, and its business rules. A "diagnosis" in the Inflammatory Disease
Context means something different from a "diagnosis" in the Motility Disorders Context, and
strategic design ensures these distinctions are explicit rather than accidental.

## Key Strategic Patterns

### Bounded Contexts

The gastroenterology domain is decomposed into six bounded contexts, each representing a
coherent clinical subdomain. These contexts are: Clinical Assessment, Endoscopic Procedures,
Inflammatory Disease, Hepatology, Motility Disorders, and Nutrition Management. Each context
has autonomy over its internal model and enforces its own invariants.

### Context Mapping

Context maps describe how bounded contexts relate to one another. In gastroenterology,
the Clinical Assessment Context acts as an upstream provider to nearly all other contexts,
since patient evaluation precedes most specialized interventions. The Endoscopic Procedures
Context has a customer-supplier relationship with the Inflammatory Disease Context, as
endoscopic findings often drive IBD management decisions.

### Subdomain Classification

Core subdomains represent the highest-value clinical activities where the system must
excel. In this domain, clinical assessment and endoscopic procedures are core. Supporting
subdomains such as nutrition management are important but do not provide primary
competitive differentiation. Generic subdomains include scheduling and general lab
result handling.

### Anti-Corruption Layers

External systems such as electronic health records, laboratory information systems, and
pharmacy management systems have their own models that do not align with the
gastroenterology domain model. Anti-corruption layers translate between external
representations and internal domain concepts, preventing model pollution.

## Application to Gastroenterology

Strategic design in gastroenterology respects the clinical reality that different
specialists, procedures, and disease processes have distinct vocabularies and workflows.
A hepatologist tracking MELD scores operates in a different conceptual space than a
motility specialist interpreting manometry tracings, even though both serve the same
patient. Strategic design ensures each context can evolve independently while
maintaining well-defined integration points.

The context map for gastroenterology reflects clinical referral patterns. A patient
presenting with abdominal pain enters through Clinical Assessment, which may trigger
an endoscopic procedure, which may reveal inflammatory disease requiring ongoing
management. Each transition crosses a bounded context boundary with explicit translation.

## Design Decisions

Strategic design decisions in gastroenterology are driven by clinical workflow analysis,
domain expert consultation, and the goal of minimizing coupling between contexts that
evolve at different rates. Hepatology treatment protocols change on different timelines
than endoscopic technology advances, and the bounded context structure accommodates this.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapters 14-15: Maintaining Model Integrity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Part I: Strategic Design.
- Brandolini, A. (2013). "Introducing EventStorming." Workshop methodology for strategic domain analysis.
