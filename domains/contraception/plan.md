# Contraception Domain - Plan

## Purpose

Apply Domain-Driven Design principles to contraception and family planning systems, creating a comprehensive model that captures contraceptive counseling, method prescribing, long-acting reversible contraception management, and patient follow-up monitoring.

## Goals

1. Establish a ubiquitous language shared among reproductive health clinicians, pharmacists, family planning counselors, patients, and system developers.
2. Define bounded contexts that reflect real-world contraception workflows from initial counseling through method selection, prescribing, and ongoing monitoring.
3. Model aggregates, entities, and value objects that represent contraception concepts such as contraceptive methods, eligibility criteria, prescriptions, and device records with accuracy.
4. Identify domain events that drive communication between bounded contexts, such as MethodPrescribed, DeviceInserted, and FollowUpScheduled.
5. Ensure compliance with WHO Medical Eligibility Criteria, FSRH guidelines, and patient safety standards is woven into domain design.

## Scope

### In Scope
- Contraceptive counseling and shared decision-making for method selection.
- Medical eligibility screening against WHO MEC categories and contraindications.
- Prescription and dispensing workflows for hormonal and barrier contraceptives.
- LARC insertion, monitoring, and removal scheduling and documentation.
- Follow-up visit tracking, side effect management, and method switching.
- Continuation and discontinuation rate monitoring.

### Out of Scope
- General administration unrelated to contraception.
- User interface design, database schema, or infrastructure implementation.
- Image file generation or diagram rendering.

## Approach

1. Strategic Design: Identify subdomains and define bounded contexts for counseling, prescribing, LARC management, and follow-up monitoring.
2. Tactical Design: Model entities, value objects, aggregates, domain events, repositories, and domain services within each bounded context.
3. Integration: Define event-driven communication patterns and anti-corruption layers between contraception services and broader clinical systems.
4. Standards and Compliance: Incorporate WHO Medical Eligibility Criteria for Contraceptive Use, FSRH Clinical Guidelines, and CDC Selected Practice Recommendations.

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
- World Health Organization. Medical Eligibility Criteria for Contraceptive Use. 5th ed. WHO, 2015.
- Faculty of Sexual and Reproductive Healthcare. FSRH Clinical Guidelines. FSRH, various years.
