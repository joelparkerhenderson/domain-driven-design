# Advance Decision to Refuse Treatment Domain - Plan

## Purpose

Apply Domain-Driven Design principles to advance decision to refuse treatment systems, creating a comprehensive model that captures patient autonomy, legal validity, clinical applicability, and stakeholder communication.

## Goals

1. Establish a ubiquitous language shared among patients, clinicians, legal professionals, social workers, and care coordinators.
2. Define bounded contexts that reflect real-world advance decision workflows from authoring through clinical application.
3. Model aggregates, entities, and value objects that represent advance decision concepts with accuracy.
4. Identify domain events that drive communication between bounded contexts.
5. Ensure compliance with the Mental Capacity Act 2005, Patient Self-Determination Act, GDPR, and HIPAA is woven into domain design.

## Scope

### In Scope

- Authoring and documenting advance decisions with specific treatment refusals.
- Assessing and recording mental capacity of the decision-maker.
- Validating advance decisions against legal requirements including witnessing and signatures.
- Applying advance decisions in clinical settings including emergency care.
- Distributing and notifying relevant parties of existing advance decisions.
- Revoking or amending advance decisions while the person retains capacity.

### Out of Scope

- General administration unrelated to advance decisions.
- User interface design, database schema, or infrastructure implementation.
- Image file generation or diagram rendering.

## Approach

1. Strategic Design: Identify subdomains (core, supporting, generic) and define bounded contexts with explicit context maps.
2. Tactical Design: Model entities, value objects, aggregates, domain events, repositories, and domain services within each bounded context.
3. Integration: Define event-driven communication patterns and anti-corruption layers between contexts.
4. Standards and Compliance: Incorporate Mental Capacity Act 2005 requirements, advance directive legislation, and healthcare data protection standards.

## Milestones

- [ ] Define bounded contexts and context map.
- [ ] Establish ubiquitous language with advance-decision-specific terminology.
- [ ] Model strategic design patterns.
- [ ] Model tactical design patterns.
- [ ] Document each bounded context in detail.
- [ ] Document cross-cutting concerns.
- [ ] Create comprehensive topic documentation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Department for Constitutional Affairs. Mental Capacity Act 2005: Code of Practice. The Stationery Office, 2007.
- Sabatino, Charles P. "The Evolution of Health Care Advance Planning Law and Policy." The Milbank Quarterly, vol. 88, no. 2, 2010.
