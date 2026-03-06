# Occupational Therapy Domain - Tasks

## Strategic Design

- [ ] Identify core, supporting, and generic subdomains for occupational therapy services.
- [ ] Define six bounded contexts aligned with occupational therapy clinical workflows.
- [ ] Create context map showing relationships between bounded contexts.
- [ ] Establish ubiquitous language with domain experts across OT practice settings.
- [ ] Design anti-corruption layers for external system integration (EHR, insurance, assistive technology vendors).

## Tactical Design

- [ ] Model aggregates for each bounded context (e.g., OccupationalProfile, InterventionPlan, ADLTrainingSession).
- [ ] Define entities with unique identities within OT workflows (e.g., Client, TherapyGoal, AdaptiveDevice, CognitiveExercise).
- [ ] Design value objects for immutable OT measurements and classifications (e.g., COPMScore, FIMRating, PerformanceSkillLevel).
- [ ] Identify domain events that cross bounded context boundaries (e.g., AssessmentCompleted, GoalAchieved, DeviceIssued, DischargeRecommended).
- [ ] Define repository abstractions for aggregate persistence.
- [ ] Design domain services for cross-aggregate operations (e.g., intervention matching, equipment recommendation, outcomes reporting).

## Documentation

- [ ] Create AGENTS.md with domain overview, bounded contexts, and principles.
- [ ] Create plan.md with goals, scope, approach, and milestones.
- [ ] Create tasks.md with comprehensive task tracking.
- [ ] Create ubiquitous-language/index.md with occupational therapy domain terminology.
- [ ] Create index.md with domain introduction, bounded contexts, and references.
