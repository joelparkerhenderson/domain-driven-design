# Psychotherapy Context

## Overview

The Psychotherapy Context is a supporting bounded context in the psychiatry domain responsible for managing therapeutic intervention delivery. This context owns the workflows for selecting evidence-based therapeutic modalities, managing therapy sessions, tracking treatment protocol adherence, assigning and reviewing homework, and monitoring the therapeutic alliance. It consumes diagnostic information from the Diagnostic Assessment Context and publishes therapy progress events to the Outcomes Measurement Context.

## Responsibility

This context answers the clinical question: "What form of psychotherapy should this patient receive, and how is it progressing?" It encompasses the full lifecycle of a therapy course from modality selection through session delivery, progress monitoring, and termination. The context does not make diagnostic decisions or prescribe medications; it receives diagnostic guidance and delivers structured therapeutic interventions.

## Ubiquitous Language

Within this context, the following terms have specific meanings:

- **Therapy Course**: A complete course of psychotherapy with a defined modality, treatment protocol, start date, goals, and expected duration.
- **Therapy Session**: A single scheduled psychotherapy appointment with documentation of interventions delivered, progress observed, and homework assigned.
- **Therapeutic Modality**: The specific type of psychotherapy being delivered (CBT, DBT, psychodynamic, motivational interviewing, EMDR, prolonged exposure).
- **Treatment Protocol**: A structured, evidence-based sequence of interventions with defined session-by-session content and progression criteria.
- **Treatment Fidelity**: The degree to which therapy delivery adheres to the evidence-based protocol for the selected modality.
- **Therapeutic Alliance**: The quality of the collaborative working relationship between therapist and patient.
- **Termination**: The planned or unplanned ending of a therapy course, with defined criteria for each termination type.

## Aggregates

### Therapy Course (Root Aggregate)

The central aggregate representing a complete course of psychotherapy.

Components:
- Modality and treatment protocol reference
- Assigned therapist reference
- Patient reference
- Diagnosis indication (from Diagnostic Assessment Context)
- Treatment goals with measurable targets
- Estimated number of sessions and expected duration
- Course status (planned, active, paused, completed, terminated early)
- Termination criteria (protocol completion, goal achievement, dropout, clinician-initiated)
- Treatment fidelity rating

Invariants:
- A therapy course must have at least one measurable treatment goal.
- The selected modality must be evidence-based for the patient's primary diagnosis.
- A course cannot be marked as completed unless termination criteria are met.
- Treatment fidelity must be assessed at least every four sessions.

### Therapy Session (Root Aggregate)

Represents a single therapy session with its clinical content and outcomes.

Components:
- Session date, time, and duration
- Session type (individual, group, family, couples)
- Session number within the therapy course
- Protocol module or session content delivered
- Specific interventions used (cognitive restructuring, behavioral activation, exposure, skills training)
- Patient engagement and participation level
- Homework review (completion status, barriers, therapeutic discussion)
- New homework assigned with rationale
- Session outcome rating (by both therapist and patient)
- Progress notes

Invariants:
- Session duration must meet minimum requirements for the associated billing code.
- Session notes must be completed within the documentation compliance window (typically 24-72 hours).
- Homework review must be documented if homework was assigned in the previous session.

## Value Objects

- **Session Duration**: Minutes and corresponding billing category.
- **Treatment Goal**: Goal description, target behavior, baseline measure, target measure, target date.
- **Homework Assignment**: Description, therapeutic rationale, due date, modality alignment.
- **Outcome Rating**: Session rating scale score (SRS), outcome rating scale score (ORS).
- **Fidelity Rating**: Protocol adherence score, competency rating, areas for improvement.
- **Intervention Type**: Intervention name, modality alignment, evidence base level.

## Domain Events

- **Therapy Course Started**: New therapy course initiated with modality and protocol selected.
- **Session Completed**: Individual session documented with interventions and outcomes.
- **Treatment Goal Achieved**: A specific treatment goal met its measurable target.
- **Alliance Rupture Detected**: Therapeutic alliance quality drops below acceptable threshold.
- **Therapy Course Completed**: Course terminated per protocol completion criteria.
- **Therapy Course Terminated Early**: Course ended before protocol completion (dropout, clinician-initiated, mutual decision).

## Domain Services

- **Treatment Protocol Recommendation Service**: Matches diagnoses and patient characteristics to evidence-based protocols.
- **Therapeutic Alliance Assessment Service**: Evaluates alliance quality using session outcome data and patient feedback.
- **Treatment Fidelity Monitoring Service**: Assesses adherence to the selected treatment protocol across sessions.

## Supported Modalities

The context supports structured modeling of the following evidence-based modalities:

- **Cognitive Behavioral Therapy (CBT)**: Structured sessions targeting cognitive distortions and behavioral patterns. Protocol includes psychoeducation, cognitive restructuring, behavioral experiments, and relapse prevention.
- **Dialectical Behavior Therapy (DBT)**: Skills-based modality with four modules (mindfulness, distress tolerance, emotion regulation, interpersonal effectiveness). Includes individual therapy, skills group, phone coaching, and consultation team.
- **Psychodynamic Therapy**: Exploration of unconscious patterns, transference, and early relational experiences. Less structured protocol with flexibility in session content.
- **Motivational Interviewing**: Brief intervention targeting ambivalence about behavior change. Structured around stages of change with specific interviewing techniques.
- **Exposure Therapy (PE/EMDR)**: Systematic exposure to trauma memories or anxiety-provoking stimuli. Highly structured protocols with defined preparation, exposure, and processing phases.

## Integration Points

- **Downstream from Diagnostic Assessment**: Receives diagnoses to guide modality selection.
- **Partnership with Rehabilitation Services**: Coordinates on patients receiving concurrent therapy and rehabilitation.
- **Downstream from Crisis Management**: Receives crisis resolution events to schedule follow-up sessions.
- **Upstream to Outcomes Measurement**: Publishes session and course events for outcome tracking.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Beck, Judith S. *Cognitive Behavior Therapy: Basics and Beyond*. Guilford Press, 2020.
- Linehan, Marsha M. *DBT Skills Training Manual*. Guilford Press, 2015.
- Shedler, Jonathan. "The Efficacy of Psychodynamic Psychotherapy." *American Psychologist*, 2010.
