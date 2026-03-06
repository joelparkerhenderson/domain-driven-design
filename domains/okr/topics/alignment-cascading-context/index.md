# Alignment & Cascading Context

## Overview

The Alignment & Cascading Context is the bounded context responsible for ensuring that OKRs at different organizational levels support and reinforce each other. It manages the hierarchical and lateral relationships between objectives, validates structural integrity of the alignment tree, detects dependency conflicts, and ensures strategic coherence from company-level objectives down to individual contributor OKRs.

Alignment is one of the most powerful aspects of the OKR methodology, as emphasized by Doerr. This context makes alignment explicit, traceable, and enforceable through domain modeling.

## Responsibilities

### Parent-Child Objective Relationships

The context models hierarchical alignment through parent-child links between objectives:

- **Company-level objectives** cascade into **division-level objectives**, which cascade into **team-level objectives**, which cascade into **individual-level objectives**.
- Each link is represented by an AlignmentLink entity within the AlignmentTree aggregate.
- A child objective's purpose is to contribute to the achievement of its parent objective. The child's key results should collectively advance the parent's key results.

Hierarchical alignment follows a tree structure: each objective may have at most one hierarchical parent, but may have multiple children. This ensures clear accountability chains.

### Cross-Team Alignment

Not all alignment is hierarchical. When multiple teams contribute to a shared outcome that does not follow the organizational hierarchy, cross-team alignment links are used:

- Cross-team links connect objectives from different teams at the same organizational level.
- These links require **bidirectional acknowledgment**: both team owners must agree to the alignment relationship.
- Cross-team links create a directed acyclic graph (DAG) overlay on top of the hierarchical tree structure.

Cross-team alignment makes dependencies visible and enables coordinated planning across organizational boundaries.

### Dependency Detection

The AlignmentValidationService performs structural validation to prevent and detect problems:

- **Cycle detection**: Ensures no circular dependencies exist in the alignment graph. A cycle would mean Objective A depends on B, B depends on C, and C depends on A, which is logically invalid.
- **Temporal validation**: Ensures a child objective's DateRange falls within or equals its parent objective's DateRange.
- **Coverage analysis**: Identifies whether child objectives collectively address the parent objective's key results, highlighting alignment gaps.
- **Conflict detection**: Flags situations where child objectives from different teams may have conflicting targets or resource contention.

### Cascading Rules

Cascading governs how top-level strategic objectives decompose into operational objectives at lower levels:

- **Top-down cascading**: Executives set company-level objectives. Division heads create supporting objectives. Teams derive their OKRs from divisional objectives.
- **Bottom-up alignment**: Individual contributors and teams can propose objectives and align them upward, enabling grass-roots innovation.
- **Bidirectional negotiation**: The recommended approach combines top-down direction with bottom-up input, where roughly 60% of objectives cascade from above and 40% originate from teams.

The cascading process integrates naturally with the OGSM (Objectives, Goals, Strategies, Measures) framework, where organizational objectives decompose into measurable strategies at each level.

## Aggregate: AlignmentTree

The AlignmentTree aggregate is the central aggregate of this context. It contains:

- **AlignmentTree** (root entity): Represents the complete alignment structure for a given OKR cycle.
- **AlignmentLink** (contained entity): Represents a single directed relationship between two objectives.
- **AlignmentType** (value object): Classifies links as Hierarchical, CrossTeam, or Supporting.

Key invariants:
- The alignment graph must be acyclic.
- An objective cannot have more than one hierarchical parent.
- Cross-team links require bidirectional acknowledgment.

## Domain Events Produced

- **AlignmentCreated**: When a new alignment link is established between objectives.
- **AlignmentChanged**: When an existing link is modified or removed.
- **AlignmentConflictDetected**: When structural validation identifies a problem.
- **CascadeValidated**: When a cascading structure passes all validation checks.
- **DependencyIdentified**: When a cross-team dependency is discovered.

## Domain Events Consumed

- **ObjectiveCreated**: From the Objective Setting Context, to suggest alignment opportunities.
- **ObjectiveUpdated**: From the Objective Setting Context, to revalidate existing alignment links.
- **ObjectiveRetired**: From the Objective Setting Context, to handle orphaned child objectives.

## Relationships to Other Topics

- **Objective Setting Context**: Objectives are defined in the [Objective Setting Context](../objective-setting-context/) and aligned here.
- **Analytics & Reporting Context**: Alignment coverage metrics feed into the [Analytics & Reporting Context](../analytics-reporting-context/).
- **Aggregates**: The AlignmentTree aggregate is documented in [Aggregates](../aggregates/).
- **Entities**: AlignmentLink entities are described in [Entities](../entities/).
- **Domain Services**: The AlignmentValidationService is detailed in [Domain Services](../domain-services/).
- **Domain Events**: Events produced and consumed are cataloged in [Domain Events](../domain-events/).
- **Strategic Design**: Alignment patterns emerge from strategic design. See [Strategic Design](../strategic-design/).
- **OKR Standards**: OGSM and cascading practices are explained in [OKR Standards](../okr-standards/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018. Chapters on alignment and transparency.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016. Chapters on cascading.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
