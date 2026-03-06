# Strategic Design for OKR Systems

## Overview

Strategic Design is the high-level discipline within Domain-Driven Design (DDD) that identifies boundaries, relationships, and organizational structures in a complex domain. Applied to Objectives and Key Results (OKR) systems, Strategic Design provides the architectural foundation for modeling how organizations set, track, align, review, and report on goals. It ensures that the OKR domain model reflects real-world organizational dynamics rather than merely technical data structures.

Strategic Design for OKR systems involves identifying bounded contexts that correspond to distinct areas of OKR practice, defining a ubiquitous language that domain experts and developers share, mapping the relationships between contexts, and classifying subdomains by their strategic importance.

## Strategic DDD Applied to OKR Systems

### Why Strategic Design Matters for OKR

OKR systems span multiple organizational concerns: executive strategy, team-level goal setting, individual performance, progress measurement, alignment across hierarchies, periodic review ceremonies, and analytical reporting. Without Strategic Design, an OKR platform risks becoming a monolithic system where changes to scoring logic inadvertently break alignment rules, or where reporting requirements contaminate the goal-setting workflow.

Strategic Design addresses this by:

- **Decomposing the domain** into bounded contexts that can evolve independently.
- **Establishing shared language** so that "Objective," "Key Result," "Score," and "Check-In" mean precisely the same thing to product managers, engineering teams, and OKR coaches.
- **Mapping integration points** so that upstream contexts (Objective Setting) and downstream contexts (Analytics & Reporting) interact through well-defined contracts.
- **Classifying subdomains** to focus investment on the core differentiating capabilities (adaptive goal-setting, scoring algorithms) while leveraging generic solutions for authentication and notifications.

### Event Storming for Goal Modeling

Event Storming is a collaborative workshop technique introduced by Alberto Brandolini that is particularly effective for modeling the OKR domain. In an Event Storming session for OKR:

1. **Domain Events Identification**: Participants identify key events such as ObjectiveCreated, KeyResultDefined, CycleOpened, CheckInRecorded, ScoreFinalized, CycleClosed, and AlignmentChanged. These events form the backbone of the domain model.

2. **Command Discovery**: Events are traced back to the commands that trigger them. For example, "Create Objective" triggers ObjectiveCreated; "Record Check-In" triggers CheckInRecorded. This reveals the user interactions and system behaviors.

3. **Aggregate Identification**: Commands and events cluster around aggregates. The Objective Aggregate owns objective creation and key result definition. The OKRCycle Aggregate manages cycle transitions. The ReviewSession Aggregate handles check-ins and scoring.

4. **Bounded Context Emergence**: Clusters of aggregates, commands, and events naturally reveal context boundaries. Goal creation and ownership concerns separate from progress tracking concerns, which separate from alignment concerns.

5. **Policy Discovery**: Policies (reactive logic) emerge between events. When ObjectiveCreated fires, a policy in the Alignment & Cascading Context checks whether the new objective aligns with parent objectives. When CycleClosed fires, policies trigger scoring finalization and retrospective scheduling.

### Distillation of the OKR Core Domain

Following Evans's guidance on Core Domain distillation, the OKR domain's strategic value concentrates in:

- **Adaptive Goal-Setting**: The ability to model committed versus aspirational OKRs, enforce SMART criteria, and support both top-down and bottom-up objective creation.
- **Scoring Algorithms**: The algorithms that translate raw key result measurements into meaningful 0.0-1.0 scores, incorporating confidence levels and weighting.
- **Alignment Intelligence**: The logic that validates cross-level alignment, detects conflicting objectives, and ensures cascading consistency.

These core capabilities differentiate the OKR system from generic task trackers or project management tools. Supporting subdomains (cascading mechanics, reporting) and generic subdomains (authentication, notifications) surround the core.

### Strategic Design Patterns in the OKR Domain

The following DDD strategic patterns apply to OKR systems:

- **Shared Kernel**: The Objective Setting Context and Key Result Tracking Context share a kernel containing the Objective entity and KeyResult entity definitions, ensuring consistency between goal definition and progress tracking.
- **Customer-Supplier**: The Objective Setting Context (upstream supplier) publishes objectives that the Alignment & Cascading Context (downstream customer) consumes to build alignment trees.
- **Conformist**: The Analytics & Reporting Context conforms to the models published by upstream contexts rather than maintaining its own translations, accepting the scoring and alignment models as-is for reporting.
- **Anti-Corruption Layer**: External integrations with HR systems, project management tools, and BI platforms are isolated behind anti-corruption layers that translate foreign models into OKR domain concepts.
- **Published Language**: The ubiquitous language is formalized as a published language for external integrators, enabling third-party tools to interact with the OKR system using standardized terminology.

### Domain Vision Statement

A Domain Vision Statement for an OKR system might read:

> This system enables organizations to set ambitious, measurable objectives and track quantifiable key results through transparent alignment across organizational levels, regular cadenced reviews, and data-driven performance analytics. It embodies the principle that what gets measured gets managed, and what gets aligned gets achieved.

## Relationships to Other Topics

- **Bounded Contexts**: Strategic Design produces the bounded context definitions detailed in the [Bounded Contexts](../bounded-contexts/) topic.
- **Context Map**: The relationships identified during Strategic Design are formalized in the [Context Map](../context-map/).
- **Ubiquitous Language**: Strategic Design establishes the need for a shared language, detailed in the [Ubiquitous Language](../ubiquitous-language/) topic.
- **Subdomains**: The core, supporting, and generic subdomain classification is a direct output of Strategic Design, documented in [Subdomains](../subdomains/).
- **Anti-Corruption Layer**: Strategic Design identifies where ACLs are needed, detailed in [Anti-Corruption Layer](../anti-corruption-layer/).
- **Aggregates**: The aggregate boundaries discovered during Event Storming are detailed in [Aggregates](../aggregates/).
- **Domain Events**: Events identified during Event Storming are formalized in [Domain Events](../domain-events/).
- **Event-Driven Architecture**: The event flows discovered during Strategic Design inform the [Event-Driven Architecture](../event-driven-architecture/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapters 14-15 on Distillation and Strategic Design.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapters 2-3 on Strategic Design and Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
