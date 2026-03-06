# Objective Setting Context

## Overview

The Objective Setting Context is the bounded context responsible for the full lifecycle of objectives and their associated key results, from initial drafting through approval, activation, and eventual completion or retirement. This context owns the canonical definition of what an Objective and Key Result are, enforces quality standards through SMART validation, and manages ownership assignment.

This context is the starting point of the OKR process. Every OKR journey begins here, with the articulation of a clear, qualitative objective supported by measurable key results.

## Responsibilities

### Objective Lifecycle Management

The Objective Setting Context governs the following lifecycle states:

**Draft**: The initial state where an Objective is being composed. Owners can freely edit the title, description, priority, and associated key results. SMART validation may be run but is not yet enforced.

**Approved**: The Objective and all its Key Results have passed SMART validation, ownership is assigned, and the objective has been reviewed by the appropriate authority (manager, OKR coach, or executive depending on organizational level).

**Active**: The Objective is being actively pursued within an open OKR cycle. Modifications are restricted to minor clarifications; structural changes (adding or removing Key Results) are not permitted.

**Scoring**: The cycle has entered its scoring phase. The Objective is locked for changes while end-of-cycle grading occurs in the Key Result Tracking Context.

**Completed**: Final scores have been recorded and the Objective is archived for historical reference.

**Retired**: The Objective was withdrawn before completion, with a recorded rationale for retirement.

### SMART Validation

Every Key Result must satisfy SMART criteria before its parent Objective can transition from Draft to Approved:

- **Specific**: The Key Result clearly defines what will be measured and what outcome is expected.
- **Measurable**: A quantitative metric with a defined baseline and target is specified.
- **Actionable**: The team or individual can influence the outcome through their actions.
- **Relatable**: The Key Result connects to the parent Objective's intent and to broader organizational goals.
- **Timely**: The measurement has a clear deadline aligned with the OKR cycle's DateRange.

SMART validation is recorded as a SMARTCriteria value object attached to each Key Result, providing an audit trail of the validation decision.

### Ownership Assignment

Each Objective must have an assigned owner before approval. Ownership determines accountability:

- **Individual ownership**: A single person is accountable for the Objective.
- **Team ownership**: A team collectively owns the Objective, with the team lead as the primary accountable party.
- **Organizational ownership**: Company-level objectives owned by executive leadership.

Ownership assignment follows role-based permissions: only OKR Coaches and Executives can create company-level objectives.

### Key Result Definition

Key Results are created within the Objective Aggregate. Each Key Result specifies:

- A description of the measurable outcome.
- A baseline (starting measurement) and target (desired end measurement).
- A measurement type (Percentage, Absolute, Binary, Currency).
- A weight indicating relative importance within the Objective (all weights must sum to 1.0).
- A data source type (Manual or Automated) indicating how progress will be tracked.

An Objective must have between 1 and 5 Key Results. This constraint, enforced as an aggregate invariant, reflects OKR best practices that recommend focus over breadth.

### OKR Type Classification

At creation, each Objective is classified as either Committed or Aspirational. This classification drives scoring expectations and accountability:

- **Committed objectives** are expected to score 1.0. Teams should allocate sufficient resources and have high confidence in delivery.
- **Aspirational objectives** are intentionally ambitious. Scores of 0.6-0.7 indicate success. These objectives encourage innovation and stretch thinking.

The OKRType is immutable after the Objective becomes Active, preventing mid-cycle reclassification that would undermine accountability.

## Domain Events Produced

- **ObjectiveCreated**: When a new Objective enters Draft state.
- **ObjectiveUpdated**: When a Draft or Approved Objective is modified.
- **ObjectiveApproved**: When an Objective passes validation and is approved.
- **ObjectiveRetired**: When an Objective is withdrawn before completion.
- **KeyResultDefined**: When a Key Result is added to an Objective.
- **KeyResultModified**: When a Key Result's definition is changed.

## Domain Events Consumed

- **CycleOpened**: From the Review & Cadence Context, signaling that objectives may be activated for a new cycle.

## Relationships to Other Topics

- **Key Result Tracking Context**: Once objectives are active, progress tracking is handled by the [Key Result Tracking Context](../key-result-tracking-context/).
- **Alignment & Cascading Context**: Objectives from this context are linked in alignment hierarchies. See [Alignment & Cascading Context](../alignment-cascading-context/).
- **Aggregates**: The Objective Aggregate is the central aggregate of this context. See [Aggregates](../aggregates/).
- **Entities**: Objective, KeyResult, and ownership entities are defined in [Entities](../entities/).
- **Value Objects**: SMARTCriteria, OKRType, Priority, and DateRange are defined in [Value Objects](../value-objects/).
- **Ubiquitous Language**: Terms like "Objective," "Key Result," "SMART," and "Committed/Aspirational" are defined in [Ubiquitous Language](../ubiquitous-language/).
- **OKR Standards**: SMART validation directly implements the SMART framework. See [OKR Standards](../okr-standards/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018. Chapters on setting effective OKRs.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
