# Domain Services

## Overview

In Domain-Driven Design, a Domain Service is a stateless operation that encapsulates domain logic which does not naturally belong to a single entity or value object. Domain services are used when a business operation spans multiple aggregates, requires coordination between bounded contexts, or represents a process that is not the responsibility of any single aggregate root.

In the OKR domain, domain services handle cross-aggregate operations such as scoring calculations that combine multiple key result measurements, alignment validation that spans the objective hierarchy, cycle state transitions that affect many aggregates, and progress aggregation across organizational levels.

## OKR Domain Services

### ScoringService

**Bounded Context**: Key Result Tracking Context

**Description**: The ScoringService calculates scores for Key Results and aggregates them into Objective-level scores. It encapsulates the scoring algorithms that translate raw measurements into the 0.0-to-1.0 scoring scale used throughout the OKR system.

**Operations**:
- `calculateKeyResultScore(keyResultId: KeyResultId): Score`: Computes a score based on baseline, target, and current value. For percentage-based Key Results, the formula is `(currentValue - baseline) / (target - baseline)`, clamped to 0.0-1.0.
- `calculateObjectiveScore(objectiveId: ObjectiveId): Score`: Computes a weighted average of all Key Result scores within an Objective, using each Key Result's weight.
- `finalizeScore(keyResultId: KeyResultId, reviewerId: PersonId): ScoreFinalized`: Locks a Key Result's score and emits a ScoreFinalized domain event.

**Business Rules**:
- Scores are clamped to the 0.0-1.0 range regardless of over-achievement.
- Binary Key Results score either 0.0 (not achieved) or 1.0 (achieved).
- Score finalization is irreversible and requires reviewer authorization.
- Aspirational objectives use a different success threshold (0.6-0.7) than committed objectives (1.0) for reporting purposes, but the raw score calculation is identical.

**Dependencies**: ObjectiveRepository (to load Key Result definitions and weights), domain event publisher (to emit ScoreFinalized).

### AlignmentValidationService

**Bounded Context**: Alignment & Cascading Context

**Description**: The AlignmentValidationService ensures structural integrity of the alignment hierarchy. It validates that proposed alignment links do not introduce cycles, that parent-child temporal constraints are satisfied, and that cross-team alignments are properly acknowledged.

**Operations**:
- `validateAlignment(parentObjectiveId: ObjectiveId, childObjectiveId: ObjectiveId): ValidationResult`: Checks whether a proposed alignment link is valid.
- `detectCycles(treeId: TreeId): List<CyclePath>`: Scans the alignment tree for circular dependencies.
- `validateCascade(parentObjectiveId: ObjectiveId): CascadeValidationResult`: Verifies that all child objectives collectively cover the parent objective's key results.
- `findUnalignedObjectives(cycleId: CycleId): List<ObjectiveId>`: Identifies objectives that are not linked to any parent objective (potential alignment gaps).

**Business Rules**:
- Circular dependencies are never permitted in alignment trees.
- A child objective's DateRange must fall within or equal its parent's DateRange.
- Cross-team links require acknowledgment from both team owners.
- Alignment gap detection is advisory, not enforced as an invariant.

**Dependencies**: AlignmentRepository, ObjectiveRepository (via anti-corruption layer for cross-context access).

### CycleTransitionService

**Bounded Context**: Review & Cadence Context

**Description**: The CycleTransitionService manages the state transitions of OKR Cycles. It enforces the ordered state machine (Planning, Active, Scoring, Closed) and coordinates the cross-context effects of each transition.

**Operations**:
- `openCycle(cycleId: CycleId): CycleOpened`: Transitions a cycle from Planning to Active and emits a CycleOpened event.
- `beginScoring(cycleId: CycleId): CycleTransitioned`: Transitions a cycle from Active to Scoring, enabling end-of-cycle grading.
- `closeCycle(cycleId: CycleId): CycleClosed`: Transitions a cycle from Scoring to Closed after verifying all scores are finalized.

**Business Rules**:
- State transitions must follow the defined order; no state may be skipped.
- A cycle cannot transition to Active before its StartDate.
- A cycle cannot transition to Closed until all associated Key Results have finalized scores.
- Cycle closure triggers archival processes and retrospective scheduling.

**Dependencies**: OKRCycleRepository, domain event publisher, ScoreFinalized event subscription (to track scoring completion).

### ProgressAggregationService

**Bounded Context**: Analytics & Reporting Context

**Description**: The ProgressAggregationService computes aggregate statistics from individual Key Result progress data. It produces team-level, department-level, and organization-level views of OKR health and achievement.

**Operations**:
- `aggregateTeamProgress(teamId: TeamId, cycleId: CycleId): TeamProgressSummary`: Computes average scores, check-in completion rates, and confidence distributions for a team.
- `aggregateOrganizationProgress(cycleId: CycleId): OrganizationProgressSummary`: Rolls up progress across all teams for executive dashboards.
- `computeTrend(objectiveId: ObjectiveId): TrendAnalysis`: Analyzes the trajectory of progress updates to project likely end-of-cycle achievement.
- `identifyAtRiskObjectives(cycleId: CycleId): List<AtRiskObjective>`: Flags objectives where confidence is Low or progress is significantly behind schedule.

**Business Rules**:
- Aggregations use the most recent finalized data; in-progress check-ins are excluded until recorded.
- Trend analysis requires at least three data points (check-ins) to produce meaningful projections.
- At-risk identification uses configurable thresholds for score and confidence level.

**Dependencies**: Read models and projections built from consumed domain events.

## Domain Service Design Principles

### Statelessness

Domain services hold no state between operations. All required data is loaded from repositories or received as parameters. This makes services easy to test, scale, and reason about.

### Domain Logic Only

Domain services contain pure business logic. They do not handle HTTP requests, database connections, or message queue management. Infrastructure concerns are handled by application services that orchestrate domain service calls.

### Named for Domain Operations

Service names and method names use ubiquitous language terminology (ScoringService, not ScoreCalculator; finalizeScore, not processScore). This keeps the domain model readable for domain experts and developers alike.

## Relationships to Other Topics

- **Aggregates**: Domain services operate across aggregates that cannot coordinate internally. See [Aggregates](../aggregates/).
- **Repositories**: Services use repositories to load and persist aggregates. See [Repositories](../repositories/).
- **Domain Events**: Services emit domain events to notify other contexts of significant outcomes. See [Domain Events](../domain-events/).
- **Value Objects**: Services produce and consume value objects such as Score and ConfidenceLevel. See [Value Objects](../value-objects/).
- **Bounded Contexts**: Each service belongs to a specific bounded context. See [Bounded Contexts](../bounded-contexts/).
- **OKR Standards**: ScoringService applies scoring methodologies from established OKR practice. See [OKR Standards](../okr-standards/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software (Services).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 7: Services.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018. Chapters on scoring methodology.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
