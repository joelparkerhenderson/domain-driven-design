# Entities in the Psychology Domain

## Overview

An entity is a domain object that has a unique identity persisting over time, even as its attributes change. In the psychology domain, entities represent the people, clinical activities, and research artifacts that must be tracked individually throughout their lifecycles. Unlike value objects, which are defined by their attributes, entities are defined by their identity. Two clients with identical demographic information are still distinct entities because each has a unique clinical history, assessment record, and treatment trajectory.

Entities carry mutable state and transition through defined lifecycle stages. A therapy session entity progresses from scheduled to in-progress to completed. An assessment entity moves from referred to administered to scored to interpreted. These state transitions are governed by business rules embedded in the entity or enforced by the aggregate root.

## Client Entity

The Client entity is central to multiple bounded contexts. In the Therapeutic Intervention Context, a Client has an identity, demographic information, presenting concerns, diagnostic history, and treatment history. The Client entity persists across sessions, treatment episodes, and even across different clinicians. A Client's identity remains constant even as their diagnosis changes, their symptoms improve, or they transfer between providers.

In each bounded context, the Client entity carries only the attributes relevant to that context. The assessment context tracks the Client's assessment history and referral information. The therapeutic context tracks treatment goals, session attendance, and progress. The behavioral analysis context tracks target behaviors and intervention response. Each context maintains its own view of the Client, linked by a shared identifier.

## Session Entity

The Session entity in the Therapeutic Intervention Context represents a discrete therapeutic encounter. It has a unique identity, a scheduled date and time, an associated therapist and client, a session type (individual, group, family), and a status (scheduled, completed, cancelled, no-show). The Session entity accumulates state over its lifecycle: pre-session measures, in-session notes, interventions applied, homework assigned, and post-session documentation.

Sessions are not interchangeable. Session 12 of a CBT protocol for depression has a specific therapeutic agenda that differs from session 5. The Session entity's identity allows tracking of protocol adherence, session-by-session outcomes, and patterns of attendance.

## Therapist Entity

The Therapist entity represents a clinical provider within the Therapeutic Intervention Context. It carries identity, credentials, licensure status, areas of specialization, and caseload information. The Therapist entity persists as credentials are renewed, specializations are added, and caseloads change. Therapist identity is essential for tracking therapeutic alliance, treatment fidelity, and clinician-level outcomes.

## Assessment Instrument Entity

In the Psychological Assessment Context, the Assessment Instrument entity represents a specific psychometric test or measure. It has an identity, a name, a publisher, edition information, applicable age ranges, available forms, and psychometric properties. The instrument entity persists as new editions are released and new normative studies are published. Tracking instrument identity is critical for ensuring that the correct version and norms are applied during scoring and interpretation.

## Participant Entity

In the Research Methods Context, the Participant entity represents an individual enrolled in a research study. It carries an identity (typically a de-identified research code), enrollment date, consent status, group assignment, and data collection status. The Participant entity is distinct from the Client entity, even when they represent the same individual, because the research context requires de-identification and uses research-specific attributes.

## Target Behavior Entity

In the Behavioral Analysis Context, the Target Behavior entity represents a specific behavior selected for assessment and intervention. It has an identity, an operational definition, a measurement method, baseline data, and intervention data. The Target Behavior persists throughout the behavior plan lifecycle, accumulating data points that track behavior change over time. Its identity allows comparison of pre-intervention and post-intervention rates.

## Cognitive Domain Entity

In the Cognitive Assessment Context, the Cognitive Domain entity represents a specific area of cognitive functioning being evaluated, such as verbal memory, visual-spatial processing, or sustained attention. It carries an identity, a definition, associated test instruments, and domain-specific scores. The entity persists across evaluation sessions, allowing longitudinal tracking of cognitive functioning within each domain.

## Entity Identity Management

In psychology practice, entities often carry multiple identifiers. A Client may have a medical record number, an insurance identifier, a research participant code, and internal system identifiers. Entity identity management must reconcile these multiple identifiers while maintaining a single authoritative identity within each bounded context. Cross-context identity mapping is handled through the context map and integration patterns, not through shared entity classes.

## Lifecycle Management

Psychology domain entities have clinically meaningful lifecycle stages that carry regulatory implications. A Session entity that transitions to "completed" triggers documentation requirements. A Participant entity that withdraws from a study triggers data handling procedures. Entity lifecycle management must enforce these clinical and regulatory rules as invariants.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on entities.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 5 on entity design.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 6 on tactical design patterns including entities.
