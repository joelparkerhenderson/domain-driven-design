# Sports Medicine Context

## Overview

The Sports Medicine Context is the bounded context responsible for managing athletic
injuries, arthroscopic procedures, and return-to-play protocols. It addresses the
unique requirements of treating athletes, where the goal extends beyond healing to
restoring competitive performance. This context models injury episodes from initial
diagnosis through definitive treatment and criteria-based sport resumption.

## Scope

This context covers:

- Athletic injury classification and severity assessment.
- Injury mechanism documentation and sport-specific risk factors.
- Arthroscopic procedure planning and intraoperative finding documentation.
- Conservative treatment protocols for non-operative injuries.
- Return-to-play protocol design and progression tracking.
- Functional testing and clearance criteria.
- Injury recurrence risk evaluation.

This context does not cover: general musculoskeletal assessment (Clinical Assessment
Context), joint replacement procedures (Joint Replacement Context), or detailed
rehabilitation therapy sessions (Rehabilitation Context).

## Ubiquitous Language

- Sports Injury Case: An athletic injury episode from onset through resolution.
- Injury Mechanism: The biomechanical cause of the injury (contact, non-contact, overuse).
- Arthroscopic Procedure: Minimally invasive surgery using a camera and instruments.
- Intraoperative Finding: A condition discovered during arthroscopy.
- Return-to-Play Protocol: A staged, criteria-based progression to sport resumption.
- Functional Test: A performance assessment measuring physical readiness.
- Clearance: Authorization to resume competitive sport after meeting all criteria.
- Competitive Level: The athlete's level of sport participation.

## Aggregate Root

**SportsInjuryCase** is the aggregate root. It ensures consistency between injury
diagnosis, treatment decisions, and return-to-play progression. An athlete cannot
advance to the next return-to-play phase until the criteria for the current phase
are met. Arthroscopic findings must be recorded before post-operative treatment
decisions are finalized.

## Key Entities

- SportsInjuryCase: Tracks the injury through assessed, treated, rehabilitating,
  and cleared states.
- AthleteProfile: Captures sport, position, competitive level, and injury history.

## Key Value Objects

- InjuryMechanism: Contact, non-contact, or overuse with descriptive detail.
- SportType: The specific sport and position or discipline.
- FunctionalTestResult: Named test, score, and pass/fail against criteria.
- ReturnToPlayPhase: Phase number, criteria, and target timeline.
- ArthroscopicFinding: Structure involved, pathology observed, and intervention applied.
- Laterality: Left, right, or bilateral.
- InjurySeverityGrade: Graded severity (e.g., Grade I-III ligament sprain).

## Domain Events Published

- SportsInjuryDiagnosed: Emitted when an athletic injury is classified.
- ArthroscopyCompleted: Emitted when an arthroscopic procedure is finished.
- ReturnToPlayPhaseAdvanced: Emitted when an athlete advances to the next phase.
- ReturnToPlayCleared: Emitted when all return criteria are met.

## Domain Events Consumed

- PatientAssessed: From Clinical Assessment, providing initial examination findings.
- DiagnosisConfirmed: From Clinical Assessment, confirming the injury diagnosis.

## Invariants

- Return-to-play phase advancement requires all current phase criteria to be met.
- Arthroscopic findings must be documented before post-operative protocol selection.
- Clearance requires all functional test criteria to be passed.
- An injured athlete cannot be cleared while any clearance criterion is unmet.
- Injury recurrence resets the return-to-play protocol to the appropriate phase.

## Domain Services

- ReturnToPlayEvaluationService: Assesses readiness for sport resumption.
- InjuryRecurrenceRiskService: Calculates re-injury risk based on clinical factors.

## Clinical Workflow

1. Athlete presents with injury; Clinical Assessment publishes PatientAssessed.
2. Sports Injury Case is created with injury classification and mechanism.
3. Treatment decision: conservative management or arthroscopic intervention.
4. If arthroscopy: procedure is performed, findings documented, ArthroscopyCompleted published.
5. Return-to-play protocol is initiated with phase-specific criteria.
6. Athlete progresses through phases, with functional testing at each gate.
7. When all criteria are met, ReturnToPlayCleared event is published.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Ardern, C.L. et al. "2016 Consensus Statement on Return to Sport." British Journal of Sports Medicine, 2016.
