# Pre-Operative Assessment Domain - Plan

## Purpose

Apply Domain-Driven Design principles to pre-operative assessment systems, creating a comprehensive model that captures patient screening, medical history and risk stratification, anaesthetic evaluation, evidence-based investigation ordering, patient optimization, and surgical readiness clearance.

## Goals

1. Establish a ubiquitous language shared among anaesthetists, surgeons, pre-assessment nurses, perioperative physicians, and software developers.
2. Define bounded contexts that reflect the real-world clinical workflows of pre-operative assessment from initial screening through surgical clearance.
3. Model aggregates, entities, and value objects that represent pre-operative assessment domain concepts with clinical accuracy, including validated risk scores and evidence-based investigation criteria.
4. Identify domain events that drive communication between bounded contexts and coordinate the progression of patients through the pre-operative pathway.
5. Ensure alignment with NICE pre-operative testing guidelines (NG45), AAGBI recommendations, Enhanced Recovery After Surgery (ERAS) protocols, and institutional clinical governance requirements.

## Scope

### In Scope

- Patient screening and triage: surgical referral processing, health questionnaire review, triage criteria application, appointment type allocation.
- Medical history and risk stratification: comprehensive history collection, ASA classification, Revised Cardiac Risk Index, surgical risk scoring, comorbidity assessment.
- Anaesthetic evaluation: airway assessment, functional capacity estimation, medication management planning, anaesthetic technique selection.
- Pre-operative investigations: evidence-based test ordering (bloods, ECG, chest X-ray, echocardiography, lung function), result interpretation, abnormal result escalation.
- Patient optimization: anaemia management, glycaemic control, blood pressure optimization, smoking cessation, prehabilitation, medication adjustment.
- Surgical readiness clearance: fitness-for-surgery determination, consent verification, fasting instruction, surgical pathway confirmation, day-of-surgery briefing.

### Out of Scope

- Intraoperative anaesthetic management and surgical procedures.
- Post-operative care and recovery beyond discharge planning initiated pre-operatively.
- User interface design, database schema, or infrastructure implementation.
- Image file generation or diagram rendering.

## Approach

1. Strategic Design: Identify subdomains (core, supporting, generic) and define bounded contexts with explicit context maps reflecting the pre-operative assessment pathway.
2. Tactical Design: Model entities, value objects, aggregates, domain events, repositories, and domain services within each bounded context.
3. Integration: Define event-driven communication patterns and anti-corruption layers between contexts and external systems (EHR, surgical scheduling, laboratory systems, blood bank).
4. Standards and Compliance: Incorporate NICE NG45 guidelines, AAGBI standards, ERAS protocols, and WHO Surgical Safety Checklist requirements into domain rules.

## Milestones

- [ ] Define bounded contexts and context map.
- [ ] Establish ubiquitous language with pre-operative assessment-specific terminology.
- [ ] Model strategic design patterns (subdomains, anti-corruption layers).
- [ ] Model tactical design patterns (aggregates, entities, value objects, domain events, repositories, domain services).
- [ ] Document each bounded context in detail.
- [ ] Document cross-cutting concerns (event-driven architecture, integration patterns, standards, compliance).
- [ ] Create comprehensive topic documentation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- National Institute for Health and Care Excellence (NICE). Routine Preoperative Tests for Elective Surgery (NG45). NICE, 2016.
- Association of Anaesthetists of Great Britain and Ireland. Pre-operative Assessment and Patient Preparation. AAGBI, 2010.
