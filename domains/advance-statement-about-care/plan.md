# Advance Statement About Care Domain - Plan

## Purpose

Apply Domain-Driven Design principles to advance statement about care systems, creating a comprehensive model that captures person-centered care preferences, values documentation, stakeholder engagement, and clinical application of wishes.

## Goals

1. Establish a ubiquitous language shared among patients, carers, clinicians, social workers, chaplains, and advocates.
2. Define bounded contexts that reflect real-world advance care planning workflows from statement authoring through clinical interpretation.
3. Model aggregates, entities, and value objects that represent advance care statement concepts with accuracy.
4. Identify domain events that drive communication between bounded contexts.
5. Ensure compliance with NHS Advance Care Planning guidance, Mental Capacity Act best interests requirements, and data protection legislation is woven into domain design.

## Scope

### In Scope

- Documenting patient wishes, preferences, values, beliefs, and feelings about future care.
- Translating stated preferences into actionable care plans.
- Engaging family members, carers, and advocates in the care planning process.
- Interpreting and applying advance statements in clinical settings.
- Reviewing and updating advance statements to reflect evolving preferences.
- Coordinating advance statements with other advance planning documents.

### Out of Scope

- General administration unrelated to advance care statements.
- User interface design, database schema, or infrastructure implementation.
- Image file generation or diagram rendering.

## Approach

1. Strategic Design: Identify subdomains (core, supporting, generic) and define bounded contexts with explicit context maps.
2. Tactical Design: Model entities, value objects, aggregates, domain events, repositories, and domain services within each bounded context.
3. Integration: Define event-driven communication patterns and anti-corruption layers between contexts.
4. Standards and Compliance: Incorporate NHS Advance Care Planning standards, person-centered care principles, and healthcare data protection requirements.

## Milestones

- [ ] Define bounded contexts and context map.
- [ ] Establish ubiquitous language with advance-care-statement-specific terminology.
- [ ] Model strategic design patterns.
- [ ] Model tactical design patterns.
- [ ] Document each bounded context in detail.
- [ ] Document cross-cutting concerns.
- [ ] Create comprehensive topic documentation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- NHS England. Universal Principles for Advance Care Planning. NHS, 2022.
- Sudore, Rebecca L., et al. "Defining Advance Care Planning for Adults: A Consensus Definition from a Multidisciplinary Delphi Panel." Journal of Pain and Symptom Management, vol. 53, no. 5, 2017.
