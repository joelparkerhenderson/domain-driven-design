# Bounded Contexts

## Overview

In Domain-Driven Design, a Bounded Context is a logical boundary within which a particular domain model is defined and applicable. Each Bounded Context has its own ubiquitous language, its own model, and its own rules. The OKR domain is decomposed into five Bounded Contexts, each representing a distinct area of responsibility in the lifecycle of objectives and key results.

Bounded Contexts prevent model confusion by ensuring that concepts like "Score" or "Objective" have precise, unambiguous meaning within each context, even if the same term carries different connotations elsewhere.

## The Five OKR Bounded Contexts

### 1. Objective Setting Context

**Purpose**: Manages the full lifecycle of objectives and their associated key results, from draft creation through approval, activation, and retirement.

**Boundaries**:
- Owns the creation, editing, approval, and archival of Objective and KeyResult entities.
- Enforces SMART criteria validation at the point of objective and key result definition.
- Manages ownership assignment (individual, team, or organizational level).
- Handles OKR type classification (committed vs. aspirational).
- Maintains the canonical definition of what an objective and key result are, including their attributes, constraints, and lifecycle states.

**Key Aggregates**: Objective Aggregate (root: Objective, contains KeyResults).

**Domain Events Produced**: ObjectiveCreated, ObjectiveUpdated, ObjectiveApproved, ObjectiveRetired, KeyResultDefined, KeyResultModified.

**Domain Events Consumed**: CycleOpened (from Review & Cadence Context, to enable objective setting for a new cycle).

**Invariants**:
- An Objective must have at least one Key Result before it can be approved.
- Each Key Result must satisfy SMART criteria.
- An Objective cannot be modified after its cycle has closed.
- Ownership must be assigned before an objective transitions from Draft to Active.

### 2. Key Result Tracking Context

**Purpose**: Manages the measurement, tracking, and scoring of key results throughout an OKR cycle.

**Boundaries**:
- Owns progress updates, check-in recordings, and confidence level assessments.
- Computes key result scores on a 0.0-to-1.0 scale.
- Integrates with KPI and KRI data sources for automated progress tracking.
- Manages the distinction between manually-reported and system-measured key results.

**Key Aggregates**: KeyResultTracking Aggregate (containing ProgressUpdates, ConfidenceLevels, Scores).

**Domain Events Produced**: CheckInRecorded, ProgressUpdated, ConfidenceChanged, ScoreCalculated, ScoreFinalized.

**Domain Events Consumed**: KeyResultDefined (from Objective Setting Context), CycleClosed (from Review & Cadence Context).

**Invariants**:
- A score must be between 0.0 and 1.0 inclusive.
- A check-in cannot be recorded for a key result outside its active cycle.
- Score finalization can only occur after a cycle is closed.
- Confidence levels must be updated at each check-in.

### 3. Alignment & Cascading Context

**Purpose**: Ensures OKRs at different organizational levels support and reinforce each other, managing parent-child relationships, cross-team dependencies, and strategic coherence.

**Boundaries**:
- Owns the alignment tree structure that links objectives across organizational levels.
- Validates that child objectives support parent objectives.
- Detects circular dependencies and conflicting alignments.
- Manages cross-team alignment links where objectives from different teams contribute to shared outcomes.
- Enforces cascading rules that govern how top-level strategic objectives decompose into team and individual OKRs.

**Key Aggregates**: AlignmentTree Aggregate (containing AlignmentLinks between Objectives).

**Domain Events Produced**: AlignmentCreated, AlignmentChanged, AlignmentConflictDetected, CascadeValidated, DependencyIdentified.

**Domain Events Consumed**: ObjectiveCreated, ObjectiveUpdated (from Objective Setting Context).

**Invariants**:
- An objective cannot align to itself (no self-referential alignment).
- Alignment trees must be acyclic (no circular dependencies).
- A child objective's cycle must fall within or equal its parent objective's cycle.
- Cross-team alignment links must be bidirectionally acknowledged.

### 4. Review & Cadence Context

**Purpose**: Manages the temporal rhythm of OKR activities, including cycle management, regular check-in schedules, retrospectives, and grading ceremonies.

**Boundaries**:
- Owns OKR cycle definitions (quarterly, annual, or custom periods).
- Manages the state machine governing cycle transitions: Planning, Active, Scoring, Closed.
- Schedules and tracks check-in cadences (weekly, biweekly).
- Organizes retrospective sessions and grading ceremonies.
- Integrates PDCA (Plan-Do-Check-Act) principles into the review process.

**Key Aggregates**: OKRCycle Aggregate, ReviewSession Aggregate.

**Domain Events Produced**: CycleOpened, CycleTransitioned, CycleClosed, CheckInScheduled, RetrospectiveScheduled, GradingCeremonyCompleted.

**Domain Events Consumed**: ScoreFinalized (from Key Result Tracking Context).

**Invariants**:
- A cycle must transition through states in order: Planning -> Active -> Scoring -> Closed.
- A cycle cannot be closed until all key results within it have finalized scores.
- Check-in cadences must fall within the active period of a cycle.
- Retrospectives can only be scheduled after a cycle enters the Scoring or Closed state.

### 5. Analytics & Reporting Context

**Purpose**: Aggregates data from all other contexts to produce dashboards, trend analyses, health metrics, and performance reports.

**Boundaries**:
- Owns read models, projections, and materialized views optimized for reporting.
- Computes aggregate statistics: average scores by team, completion rates, alignment coverage, score trends over time.
- Integrates SWOT and PEST analyses with OKR performance data.
- Produces health metrics that indicate organizational OKR maturity.
- Generates exportable reports for executive reviews and board presentations.

**Key Aggregates**: This context primarily uses read models rather than traditional aggregates, following CQRS principles.

**Domain Events Consumed**: ObjectiveCreated, ScoreFinalized, CycleClosed, AlignmentChanged, CheckInRecorded (from all upstream contexts).

**Domain Events Produced**: ReportGenerated, AlertTriggered (when health metrics breach thresholds).

**Invariants**:
- Reports must reflect the most recent finalized data (eventual consistency acceptable with bounded staleness).
- Dashboard metrics must be traceable to source events.
- Trend analyses require at least two completed cycles of data.

## Context Boundary Design Principles

The five bounded contexts follow several important design principles:

1. **Single Responsibility**: Each context owns a distinct concern. Objective creation is separate from progress tracking, which is separate from alignment management.

2. **Autonomy**: Each context can evolve independently. Changing the scoring algorithm in the Key Result Tracking Context does not require changes to the Objective Setting Context.

3. **Explicit Contracts**: Contexts communicate through well-defined domain events and, where necessary, through published APIs with shared contracts.

4. **Eventual Consistency**: Cross-context consistency is achieved through domain events and eventual consistency rather than distributed transactions.

5. **Separate Data Ownership**: Each context owns its data store. The Analytics & Reporting Context maintains its own read-optimized projections rather than querying other contexts' databases directly.

## Relationships to Other Topics

- **Strategic Design**: Bounded contexts emerge from the Strategic Design process detailed in [Strategic Design](../strategic-design/).
- **Context Map**: Relationships between these five contexts are formally mapped in [Context Map](../context-map/).
- **Ubiquitous Language**: Each bounded context defines its own dialect of the ubiquitous language, as described in [Ubiquitous Language](../ubiquitous-language/).
- **Subdomains**: Bounded contexts align with subdomains; the mapping is detailed in [Subdomains](../subdomains/).
- **Aggregates**: Each context contains specific aggregates documented in [Aggregates](../aggregates/).
- **Entities**: The entities within each context are described in [Entities](../entities/).
- **Domain Events**: Events produced and consumed by each context are detailed in [Domain Events](../domain-events/).
- **Event-Driven Architecture**: The event-based communication between contexts is elaborated in [Event-Driven Architecture](../event-driven-architecture/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
