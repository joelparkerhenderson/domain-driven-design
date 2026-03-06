# Strategic Design in the Mental Health Domain

## Overview

Strategic design in Domain-Driven Design addresses how to decompose a large,
complex system into manageable parts that can evolve independently while
maintaining coherent collaboration. In the mental health domain, strategic
design is particularly important because the field spans multiple professional
disciplines (psychiatry, psychology, social work, nursing), regulatory
frameworks (HIPAA, 42 CFR Part 2), and clinical workflows that often overlap
but carry distinct semantics.

The strategic design patterns applied to this domain include bounded contexts,
context maps, subdomain classification, and anti-corruption layers. Together
these patterns create a blueprint for how mental health system components
relate to one another while preserving the integrity of each component's
internal model.

## Why Strategic Design Matters for Mental Health

Mental health care involves concepts that appear similar across contexts but
carry different meanings. The word "assessment" means one thing in the Clinical
Assessment Context (a structured diagnostic evaluation) and something different
in the Outcomes Measurement Context (a repeated symptom measure). Without
strategic design, these semantic differences collapse into ambiguous shared
models that confuse both clinicians and developers.

Strategic design forces explicit decisions about where concepts live, how
contexts communicate, and which parts of the system deserve the most investment.
A crisis intervention workflow has fundamentally different quality attributes
(speed, availability, protocol rigidity) than outcomes reporting (accuracy,
longitudinal analysis, aggregation). Strategic design ensures these differences
are reflected in the system architecture.

## Bounded Contexts

The mental health domain is decomposed into six bounded contexts:

1. Clinical Assessment Context - owns diagnostic evaluation and screening
2. Treatment Planning Context - owns individualized care planning
3. Therapy Management Context - owns session delivery and documentation
4. Crisis Intervention Context - owns emergency response workflows
5. Medication Management Context - owns prescribing and pharmacotherapy
6. Outcomes Measurement Context - owns longitudinal outcome tracking

Each context maintains its own model, its own ubiquitous language subset, and
its own consistency rules. A "patient" in the Crisis Intervention Context
carries risk factors and safety plan references, while the same individual in
the Medication Management Context carries medication history and allergy
information.

## Context Map

The context map documents how these six bounded contexts interact. Key
relationships include:

- Clinical Assessment publishes diagnostic results that Treatment Planning
  consumes to build individualized plans (customer-supplier).
- Treatment Planning directs Therapy Management by specifying modalities and
  goals (customer-supplier).
- Crisis Intervention operates as an autonomous context that can preempt
  workflows in any other context when a patient enters crisis.
- Medication Management and Therapy Management share a published language
  around treatment modalities and side effect monitoring.
- Outcomes Measurement subscribes to events from all other contexts to
  aggregate longitudinal data.

## Subdomain Classification

- Core subdomains: Clinical Assessment and Treatment Planning, because
  accurate diagnosis and individualized planning are the competitive
  differentiators of any mental health system.
- Supporting subdomains: Therapy Management and Crisis Intervention, which
  are essential but follow more standardized protocols.
- Generic subdomains: Medication Management (leverages pharmacy standards)
  and Outcomes Measurement (applies general measurement science).

## Anti-Corruption Layers

Anti-corruption layers protect bounded contexts from external systems whose
models do not align with the domain. In the mental health domain, ACLs are
needed at the boundaries with electronic health record (EHR) systems, pharmacy
benefit managers, insurance claims processors, and external crisis hotlines.
These layers translate external concepts into the domain's ubiquitous language
and prevent foreign terminology from contaminating internal models.

## Application to Mental Health

Strategic design enables mental health systems to:

- Evolve clinical assessment instruments independently of treatment planning.
- Introduce new therapeutic modalities without disrupting medication workflows.
- Scale crisis intervention infrastructure independently for high availability.
- Comply with different regulatory requirements per context boundary.
- Onboard new clinical staff using context-specific language rather than
  system-wide jargon.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapters 14-16 on strategic design.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on domains, subdomains, and bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Part II on strategic design decisions.
- American Psychiatric Association. Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition (DSM-5). APA Publishing, 2013.
