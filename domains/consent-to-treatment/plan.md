# Consent to Treatment Domain - Plan

## Purpose

Apply Domain-Driven Design principles to consent-to-treatment systems, creating a comprehensive model that captures informed consent workflows, capacity assessment, proxy decision-making, and consent lifecycle management in healthcare settings.

## Goals

1. Establish a ubiquitous language shared among clinicians, patients, legal advisors, ethics committees, and healthcare administrators.
2. Define bounded contexts that reflect real-world consent workflows from information disclosure through authorization and ongoing management.
3. Model aggregates, entities, and value objects that represent consent concepts such as consent forms, capacity evaluations, treatment disclosures, and authorization records with accuracy.
4. Identify domain events that drive communication between bounded contexts, such as ConsentGranted, CapacityAssessed, and ConsentWithdrawn.
5. Ensure compliance with the Mental Capacity Act, GMC guidelines, and the common law doctrine of informed consent is woven into domain design.

## Scope

### In Scope
- Information disclosure and patient education about proposed treatments, risks, benefits, and alternatives.
- Capacity assessment workflows for determining a patient's ability to make treatment decisions.
- Proxy and surrogate decision-making when a patient lacks capacity.
- Consent record creation, verification, renewal, modification, withdrawal, and archival.
- Audit trail and traceability for all consent-related activities.
- Emergency treatment authorization when consent cannot be obtained.

### Out of Scope
- General administration unrelated to consent to treatment.
- User interface design, database schema, or infrastructure implementation.
- Image file generation or diagram rendering.

## Approach

1. Strategic Design: Identify subdomains and define bounded contexts for informed consent, capacity assessment, proxy decision-making, and consent lifecycle management.
2. Tactical Design: Model entities, value objects, aggregates, domain events, repositories, and domain services within each bounded context.
3. Integration: Define event-driven communication patterns and anti-corruption layers between consent and clinical systems.
4. Standards and Compliance: Incorporate the Mental Capacity Act 2005, GMC Decision Making and Consent guidelines, and the ethical principles of autonomy, beneficence, non-maleficence, and justice.

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
- Beauchamp, Tom L., and James F. Childress. Principles of Biomedical Ethics. 8th ed. Oxford University Press, 2019.
- General Medical Council. Decision Making and Consent. GMC, 2020.
