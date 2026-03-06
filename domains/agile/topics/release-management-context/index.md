# Release Management Context in Agile Management

## Overview

The Release Management Context is the bounded context responsible for release planning, deployment coordination, feature flag management, semantic versioning, CI/CD integration, and rollback procedures. This context bridges the gap between completed sprint increments and production delivery, ensuring that value reaches users reliably and safely. It manages the last mile of agile delivery -- transforming verified increments into deployed releases with proper versioning, controlled rollout, and rollback capability.

## Relevance to Agile Management

Agile emphasizes delivering working software frequently. However, "potentially shippable increment" and "actually shipped to production" are different concerns with different risks and stakeholders. The Release Management Context provides the structure for deciding when to release, what to include, how to deploy safely, and how to recover if something goes wrong. Without this context, teams either release chaotically (risking production stability) or delay releases indefinitely (undermining agile's delivery promise).

## Core Responsibilities

### Release Planning

Release planning determines which completed increments are bundled into a release, when the release will ship, and what deployment targets it covers. Release planning can follow several cadences:

- **Continuous Delivery**: Every completed increment is automatically deployed to production
- **Sprint-Aligned Releases**: A release occurs at the end of each sprint
- **Release Train**: Multiple teams synchronize their releases on a fixed cadence (e.g., quarterly)
- **Feature-Based Releases**: Releases ship when a specific feature set is complete, regardless of sprint boundaries

### Feature Flags

Feature flags (also called feature toggles) decouple deployment from release. Code is deployed to production but new functionality is hidden behind a flag that can be toggled on or off without redeployment. Feature flags enable:

- **Gradual Rollout**: Enabling features for a percentage of users or specific user segments
- **A/B Testing**: Comparing user behavior between flag-on and flag-off cohorts
- **Kill Switches**: Instantly disabling a feature if it causes problems in production
- **Trunk-Based Development**: Merging incomplete features safely because they are behind flags

Every feature flag must have an owner (the person responsible for the flag lifecycle) and an expiration policy (the date by which the flag should be removed or made permanent). Orphaned flags accumulate as technical debt.

### Deployment

Deployment is the technical process of moving a release candidate to a target environment. Deployment strategies include:

- **Blue/Green Deployment**: Two identical environments; traffic is switched from old (blue) to new (green)
- **Canary Deployment**: New version serves a small percentage of traffic before full rollout
- **Rolling Deployment**: Instances are updated incrementally across a cluster

### Versioning

Releases follow semantic versioning (SemVer 2.0.0): MAJOR.MINOR.PATCH. Major version increments signal breaking changes, minor version increments add functionality in a backward-compatible manner, and patch version increments address backward-compatible bug fixes. Pre-release versions (e.g., 2.4.0-rc.1) identify release candidates.

### CI/CD Integration

Continuous Integration ensures that code changes are automatically built and tested. Continuous Delivery extends this by automatically preparing release candidates. The Release Management Context integrates with CI/CD pipelines to verify that all quality gates (unit tests, integration tests, security scans, performance benchmarks) pass before a release candidate is promoted to production.

## Aggregate Roots

- **Release**: The primary aggregate root, containing release items, feature flags, deployment history, and rollback plans

## Key Invariants

- A release must contain at least one completed increment
- All release items must meet the Definition of Done
- Feature flags must have an owner and an expiration policy
- Rollback procedures must be defined and verified before any production deployment
- Release versions must follow semantic versioning rules (MAJOR.MINOR.PATCH)
- A release version must be unique across all releases

## Domain Events Produced

- ReleasePlanned -- A release scope and timeline are defined
- ReleaseCandidateCreated -- A release candidate is assembled from completed increments
- ReleaseDeployed -- A release is successfully deployed to a target environment
- ReleaseRolledBack -- A deployed release is reverted to the previous version
- FeatureFlagToggled -- A feature flag state is changed in a target environment

## Domain Events Consumed

- SprintCompleted (from Sprint Execution) -- Evaluates whether new increments trigger a release candidate
- StoryCompleted (from Sprint Execution) -- Adds completed items to the release candidate pool
- BacklogRefined (from Backlog Management) -- Informs release scope planning for upcoming releases

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Release Management is one of the five agile bounded contexts
- [Sprint Execution Context](../sprint-execution-context/) -- Provides completed increments that become release items
- [Continuous Improvement Context](../continuous-improvement-context/) -- Receives deployment data for lead time calculation
- [Aggregates](../aggregates/) -- Release is the aggregate root with ReleaseItem and FeatureFlag as internal entities
- [Value Objects](../value-objects/) -- SemanticVersion, ReleaseNotes, DeploymentTarget, and RollbackPlan are value objects
- [Domain Events](../domain-events/) -- ReleaseDeployed communicates delivery milestones to other contexts

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." Section: Increment. 2020.
- Humble, Jez and Farley, David. "Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation." Addison-Wesley, 2010.
- Cohn, Mike. "Agile Estimating and Planning." Chapters 19-20: Release Planning. Prentice Hall, 2005.
- Hodgson, Pete and Fowler, Martin. "Feature Toggles (aka Feature Flags)." martinfowler.com, 2017.
