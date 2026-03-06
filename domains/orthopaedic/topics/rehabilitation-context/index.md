# Rehabilitation Context

## Overview

The Rehabilitation Context is the bounded context responsible for managing post-operative
and post-injury recovery programs. It encompasses physical therapy, occupational therapy,
protocol-driven rehabilitation, milestone-based progress tracking, and patient-reported
outcome measurement. This context translates surgical and clinical outcomes into
structured recovery pathways.

## Scope

This context covers:

- Post-operative rehabilitation protocol selection and application.
- Physical therapy session planning and documentation.
- Occupational therapy for upper extremity and functional recovery.
- Functional milestone definition and achievement tracking.
- Range of motion and strength progression monitoring.
- Weight-bearing status management and advancement.
- Patient-reported outcome measure collection.
- Discharge criteria evaluation.

This context does not cover: the surgical procedure itself (Joint Replacement,
Fracture Management, or Sports Medicine Contexts), pre-operative assessment
(Surgical Planning Context), or initial diagnosis (Clinical Assessment Context).

## Ubiquitous Language

- Rehabilitation Episode: A complete course of post-operative or post-injury therapy.
- Therapy Phase: A defined stage with specific goals and permitted activities.
- Functional Milestone: A measurable achievement marking recovery progress.
- Therapy Session: A single appointment with exercises, measurements, and observations.
- Range of Motion Target: The joint movement goal for a specific phase.
- Weight-Bearing Progression: Staged increase in permitted load on a limb.
- Discharge Criteria: The conditions that must be met to end rehabilitation.
- Protocol: A standardized plan defining phases, milestones, and timelines.
- Patient-Reported Outcome: A questionnaire measuring patient-perceived health status.

## Aggregate Root

**RehabilitationEpisode** is the aggregate root. It maintains consistency across
therapy phases, functional milestones, and outcome measures. Phases must be completed
sequentially. Milestones cannot be marked achieved without supporting assessment data.
Weight-bearing progression must comply with the surgical team's prescription.

## Key Entities

- RehabilitationEpisode: Tracks the full course from initiation to discharge.
- TherapySession: Records an individual therapy appointment.

## Key Value Objects

- TherapyPhase: Phase number, name, duration, goals, and activity restrictions.
- FunctionalMilestone: Milestone name, target measurement, and achievement criteria.
- RangeOfMotionTarget: Joint, plane of motion, and target angle in degrees.
- WeightBearingStatus: Non-weight-bearing, toe-touch, partial, weight-bearing as tolerated, full.
- ExercisePrescription: Exercise name, sets, repetitions, resistance, and frequency.
- VASPainScore: Visual analog scale measurement (0-100).
- OxfordHipScore: Patient-reported outcome (0-48).
- OxfordKneeScore: Patient-reported outcome (0-48).
- WOMACScore: Pain, stiffness, and function subscales.

## Domain Events Published

- RehabilitationEpisodeStarted: Emitted when a rehabilitation program begins.
- TherapyPhaseAdvanced: Emitted when a patient progresses to the next phase.
- MilestoneAchieved: Emitted when a functional milestone is reached.
- WeightBearingAdvanced: Emitted when weight-bearing status is increased.
- RehabilitationCompleted: Emitted when all discharge criteria are met.

## Domain Events Consumed

- ArthroplastyCompleted: From Joint Replacement, triggering post-arthroplasty protocol.
- FractureFixated: From Fracture Management, triggering post-fixation protocol.
- ArthroscopyCompleted: From Sports Medicine, triggering post-arthroscopy protocol.
- FractureUnionConfirmed: From Fracture Management, enabling weight-bearing advancement.
- ReturnToPlayCleared: From Sports Medicine, coordinating discharge.

## Invariants

- Therapy phases must be completed in sequential order.
- Milestone achievement requires documented assessment data meeting criteria.
- Weight-bearing progression must not exceed the surgical team's prescription.
- Outcome measures must be collected at protocol-defined intervals.
- Discharge requires all discharge criteria to be met.
- Session documentation must include exercises performed and measurements taken.

## Domain Services

- ProtocolSelectionService: Determines the appropriate protocol based on procedure
  and patient factors.
- FunctionalProgressService: Evaluates progress against protocol targets.

## Clinical Workflow

1. Surgical completion event (ArthroplastyCompleted, FractureFixated, or
   ArthroscopyCompleted) triggers episode creation.
2. ProtocolSelectionService determines the appropriate rehabilitation protocol.
3. RehabilitationEpisodeStarted event is published.
4. Therapy sessions are conducted with exercises and measurements documented.
5. Milestones are evaluated at each session; MilestoneAchieved when criteria are met.
6. Phases advance as phase-specific goals are completed.
7. Weight-bearing status progresses per surgical prescription and healing evidence.
8. Outcome scores are collected at protocol intervals.
9. Discharge criteria are evaluated; RehabilitationCompleted when all are met.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Physical Therapy Association. Clinical Practice Guidelines. https://www.apta.org
