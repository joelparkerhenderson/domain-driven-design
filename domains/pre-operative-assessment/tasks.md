# Pre-Operative Assessment Domain - Tasks

## Strategic Design

- [ ] Identify core, supporting, and generic subdomains for pre-operative assessment services.
- [ ] Define six bounded contexts aligned with pre-operative assessment clinical workflows.
- [ ] Create context map showing relationships between bounded contexts.
- [ ] Establish ubiquitous language with domain experts across anaesthesia, surgery, and nursing.
- [ ] Design anti-corruption layers for external system integration (EHR, surgical scheduling, laboratory, blood bank).

## Tactical Design

- [ ] Model aggregates for each bounded context (e.g., PreAssessmentCase, RiskProfile, AnaestheticPlan, InvestigationOrder).
- [ ] Define entities with unique identities within pre-operative workflows (e.g., Patient, SurgicalProcedure, Investigation, OptimizationTarget).
- [ ] Design value objects for immutable pre-operative measurements and classifications (e.g., ASAClassification, MallampatiGrade, RCRIScore, FunctionalCapacity).
- [ ] Identify domain events that cross bounded context boundaries (e.g., TriageCompleted, RiskStratified, InvestigationResultReceived, OptimizationTargetMet, ClearedForSurgery).
- [ ] Define repository abstractions for aggregate persistence.
- [ ] Design domain services for cross-aggregate operations (e.g., investigation recommendation engine, risk score calculation, optimization protocol assignment).

## Documentation

- [ ] Create AGENTS.md with domain overview, bounded contexts, and principles.
- [ ] Create plan.md with goals, scope, approach, and milestones.
- [ ] Create tasks.md with comprehensive task tracking.
- [ ] Create ubiquitous-language/index.md with pre-operative assessment domain terminology.
- [ ] Create index.md with domain introduction, bounded contexts, and references.
