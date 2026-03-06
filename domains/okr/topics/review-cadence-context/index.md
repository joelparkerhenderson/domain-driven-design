# Review & Cadence Context

## Overview

The Review & Cadence Context is the bounded context responsible for managing the temporal rhythm of OKR activities. It owns the OKR cycle lifecycle, regular check-in schedules, retrospective sessions, and grading ceremonies. This context ensures that OKR practice maintains consistent cadence and that the organization follows a disciplined cycle of planning, execution, review, and learning.

The review cadence is where the PDCA (Plan-Do-Check-Act) cycle directly integrates with OKR practice, creating a continuous improvement loop that drives organizational learning.

## Responsibilities

### OKR Cycles

OKR cycles are the temporal containers within which all OKR activities occur. The most common cadence is quarterly, though organizations may use different time frames:

- **Quarterly cycles**: The standard OKR cadence, providing enough time for meaningful progress while maintaining urgency. Most operational objectives use quarterly cycles.
- **Annual cycles**: Used for strategic, company-level objectives that span a full year. Annual objectives typically have quarterly key results or milestones.
- **Custom cycles**: Some organizations use half-year or trimester cycles based on their business rhythm.

Each cycle follows a strict state machine: **Planning** (objectives are drafted and approved), **Active** (objectives are pursued and progress is tracked), **Scoring** (end-of-cycle grading occurs), **Closed** (final scores are recorded and the cycle is archived).

### Check-In Cadence

Regular check-ins maintain momentum and enable course correction during the active phase of a cycle:

- **Weekly check-ins**: Brief progress updates focused on current value, confidence level, and immediate blockers. These are lightweight and typically take 15-30 minutes per team.
- **Biweekly check-ins**: A less frequent alternative for organizations that prefer a lighter touch, or for stable Key Results that do not change rapidly.
- **Monthly reviews**: Deeper progress reviews that include trend analysis, resource reallocation discussions, and strategic reassessment.

The check-in cadence is defined as a Cadence value object within the OKRCycle aggregate and generates a schedule of ReviewSession instances.

### Retrospectives

Retrospectives occur during the Scoring or Closed phase of a cycle and serve as the "Act" step in the PDCA cycle:

- **What went well**: Key Results that were achieved or exceeded, effective practices, and successful collaboration.
- **What could improve**: Objectives that fell short, process inefficiencies, and misalignments discovered during the cycle.
- **Action items**: Concrete improvements to carry forward into the next cycle's planning phase.

Retrospective findings directly inform the next cycle's objective setting, creating a feedback loop between the Review & Cadence Context and the Objective Setting Context.

### Grading Ceremonies

End-of-cycle grading is a formal process where Key Result scores are reviewed, validated, and finalized:

- Grading sessions review each Key Result's progress history, final measurement, and score.
- Scores are discussed in the context of whether the objective was Committed or Aspirational.
- The grading ceremony produces ScoreFinalized events that enable cycle closure.
- Grading sessions may also surface learnings about objective quality, measurement effectiveness, and scoring calibration.

### PDCA Integration

The PDCA (Plan-Do-Check-Act) cycle maps directly to OKR cadence:

- **Plan**: During the Planning phase, teams set objectives and define key results (Objective Setting Context).
- **Do**: During the Active phase, teams execute toward their key results (Key Result Tracking Context).
- **Check**: During check-ins and mid-cycle reviews, teams assess progress and confidence (Review & Cadence Context).
- **Act**: During retrospectives and cycle transitions, teams adjust processes and carry learnings forward (Review & Cadence Context).

This integration ensures that OKR practice is not merely a goal-setting exercise but a continuous improvement methodology.

## Aggregates

This context contains two aggregates:

- **OKRCycle Aggregate**: Manages cycle state, DateRange, and CheckInCadence.
- **ReviewSession Aggregate**: Manages individual review sessions with their CheckInEntries.

Both aggregates are documented in detail in [Aggregates](../aggregates/).

## Domain Events Produced

- **CycleOpened**: When a cycle transitions from Planning to Active.
- **CycleTransitioned**: When a cycle changes state.
- **CycleClosed**: When a cycle transitions to the Closed state.
- **CheckInScheduled**: When a check-in session is created based on the cadence.
- **RetrospectiveScheduled**: When a retrospective session is planned.
- **GradingCeremonyCompleted**: When all scores in a grading session are finalized.

## Domain Events Consumed

- **ScoreFinalized**: From the Key Result Tracking Context, used to determine if all scores are in and the cycle can close.

## Relationships to Other Topics

- **Objective Setting Context**: Cycle planning triggers objective setting. See [Objective Setting Context](../objective-setting-context/).
- **Key Result Tracking Context**: Check-ins drive progress tracking. See [Key Result Tracking Context](../key-result-tracking-context/).
- **Analytics & Reporting Context**: Cycle data feeds into reporting. See [Analytics & Reporting Context](../analytics-reporting-context/).
- **Aggregates**: OKRCycle and ReviewSession aggregates are detailed in [Aggregates](../aggregates/).
- **Entities**: OKRCycle and ReviewSession entities are described in [Entities](../entities/).
- **Domain Services**: CycleTransitionService is documented in [Domain Services](../domain-services/).
- **Value Objects**: DateRange and Cadence are defined in [Value Objects](../value-objects/).
- **OKR Standards**: PDCA integration is explained in [OKR Standards](../okr-standards/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018. Chapters on cadence and review.
- Deming, W. Edwards. "Out of the Crisis." MIT Press, 1986. Foundation of the PDCA cycle.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
