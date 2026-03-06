# Mobility Domain - Tasks

## Strategic Design

- [ ] Identify core, supporting, and generic subdomains for mobility services.
- [ ] Define five bounded contexts aligned with mobility clinical workflows.
- [ ] Create context map showing relationships between bounded contexts.
- [ ] Establish ubiquitous language with domain experts across rehabilitation disciplines.
- [ ] Design anti-corruption layers for external system integration (EHR, insurance, device vendors).

## Tactical Design

- [ ] Model aggregates for each bounded context (e.g., MobilityAssessment, DevicePrescription, GaitStudy).
- [ ] Define entities with unique identities within mobility workflows (e.g., Patient, AssistiveDevice, FallEvent).
- [ ] Design value objects for immutable mobility measurements and classifications (e.g., BalanceScore, GaitParameter, FIMScore).
- [ ] Identify domain events that cross bounded context boundaries (e.g., AssessmentCompleted, DevicePrescribed, FallRiskIdentified).
- [ ] Define repository abstractions for aggregate persistence.
- [ ] Design domain services for cross-aggregate operations (e.g., device recommendation engine, fall risk stratification).

## Documentation

- [ ] Create AGENTS.md with domain overview, bounded contexts, and principles.
- [ ] Create plan.md with goals, scope, approach, and milestones.
- [ ] Create tasks.md with comprehensive task tracking.
- [ ] Create ubiquitous-language/index.md with mobility domain terminology.
- [ ] Create index.md with domain introduction, bounded contexts, and references.
