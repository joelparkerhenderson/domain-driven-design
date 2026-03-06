# Dentistry Domain - Plan

## Purpose

Apply Domain-Driven Design principles to dentistry and oral healthcare systems, creating a comprehensive model that captures clinical examination, treatment planning, dental procedures, preventive care, and patient management workflows.

## Goals

1. Establish a ubiquitous language shared among dentists, dental hygienists, dental nurses, orthodontists, oral surgeons, and system developers.
2. Define bounded contexts that reflect real-world dental workflows from examination and diagnosis through treatment planning, procedure execution, and preventive care.
3. Model aggregates, entities, and value objects that represent dental concepts such as dental charts, treatment plans, procedures, and recall schedules with accuracy.
4. Identify domain events that drive communication between bounded contexts, such as ExaminationCompleted, TreatmentPlanApproved, and ProcedurePerformed.
5. Ensure compliance with ADA coding standards, FDI dental notation, and clinical practice guidelines is woven into domain design.

## Scope

### In Scope
- Patient intake, dental charting, and clinical examination workflows.
- Treatment plan creation, sequencing, costing, and patient authorization.
- Documentation of restorative, endodontic, periodontal, prosthodontic, orthodontic, and oral surgical procedures.
- Preventive care including prophylaxis, fluoride treatments, sealants, and recall scheduling.
- Dental imaging ordering, interpretation, and record management.
- Periodontal assessment including probing depths, attachment levels, and disease classification.

### Out of Scope
- General administration unrelated to dentistry.
- User interface design, database schema, or infrastructure implementation.
- Image file generation or diagram rendering.

## Approach

1. Strategic Design: Identify subdomains and define bounded contexts for clinical examination, treatment planning, dental procedures, and preventive care.
2. Tactical Design: Model entities, value objects, aggregates, domain events, repositories, and domain services within each bounded context.
3. Integration: Define event-driven communication patterns and anti-corruption layers between dental and broader healthcare systems.
4. Standards and Compliance: Incorporate ADA Current Dental Terminology (CDT), FDI World Dental Federation notation, and evidence-based clinical practice guidelines.

## Milestones

- [ ] Define bounded contexts and context map.
- [ ] Establish ubiquitous language.
- [ ] Model strategic design patterns.
- [ ] Model tactical design patterns.
- [ ] Document each bounded context.
- [ ] Document cross-cutting concerns.
- [ ] Create comprehensive topic documentation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- American Dental Association. CDT: Code on Dental Procedures and Nomenclature. ADA, published annually.
- Summitt, James B., et al. Fundamentals of Operative Dentistry: A Contemporary Approach. 4th ed. Quintessence, 2013.
