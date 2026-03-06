# Strategic Design in Agile Management

## Overview

Strategic design in Domain-Driven Design (DDD) addresses how to decompose a complex agile management system into manageable parts that align with how agile teams actually work. It focuses on identifying the most important areas of the agile domain, defining clear boundaries between subsystems, and establishing how those subsystems communicate. When applied to agile management, strategic design leverages techniques like Event Storming to model sprint workflows, backlog refinement ceremonies, and release processes as first-class domain concepts.

## Relevance to Agile Management

Agile management systems span multiple concerns: backlog prioritization, sprint execution, team capacity planning, continuous improvement, and release coordination. Without strategic design, these concerns bleed into one another, creating monolithic tools that resist change -- the very antithesis of agility. Strategic design provides the tools to manage this complexity by:

- Aligning software models with how Product Owners, Scrum Masters, and Development Teams actually think about their work
- Separating concerns so that changes in release planning do not ripple into sprint execution logic
- Identifying which parts of the system deserve the most investment (sprint optimization, velocity prediction) versus which can use off-the-shelf solutions (authentication, notifications)
- Enabling independent teams to work on different bounded contexts without constant coordination

## Key Concepts

### Knowledge Crunching

Knowledge crunching is the collaborative process of extracting domain knowledge from agile practitioners and translating it into a software model. In agile management, this means:

- Conducting workshops with Product Owners, Scrum Masters, developers, and stakeholders
- Observing agile ceremonies such as sprint planning, daily standups, retrospectives, and backlog refinement sessions
- Iteratively refining the model as understanding deepens
- Resolving ambiguities in terminology (e.g., "story" means different things in the Backlog Management Context versus the Sprint Execution Context)

### Event Storming for Agile Processes

Event Storming is particularly well-suited to modeling agile workflows because agile is inherently event-driven. Key event storming sessions for agile management include:

- **Sprint lifecycle flow**: SprintPlanned, SprintStarted, DailyStandupHeld, SprintReviewed, SprintRetrospected, SprintCompleted
- **Backlog flow**: EpicCreated, StoryWritten, StorySplit, StoryEstimated, StoryPrioritized, StoryRefined
- **Release flow**: ReleasePlanned, FeatureCompleted, ReleaseCandidate, ReleaseDeployed, ReleaseRolledBack
- **Improvement flow**: RetroFacilitated, ActionItemIdentified, ActionItemAssigned, ImprovementImplemented

### Domain Vision Statement

A domain vision statement captures the core purpose and value of the system. For an agile management system:

> "To provide a unified platform that supports iterative, incremental delivery by modeling sprint execution, backlog management, team capacity, continuous improvement, and release coordination as first-class domain concepts, enabling agile teams to focus on delivering value rather than managing process overhead."

### Core Domain Identification

Not all parts of the agile management system are equally important. Strategic design distinguishes:

- **Core Domain**: Sprint optimization, velocity prediction, and capacity planning -- the areas that differentiate the system and directly impact team effectiveness
- **Supporting Subdomains**: Backlog management, release planning, retrospective facilitation -- necessary but not the primary differentiator
- **Generic Subdomains**: Authentication, notifications, user management, audit logging -- commoditized capabilities available as off-the-shelf solutions

### Distillation

Distillation separates the essential complexity of the core domain from accidental complexity. In agile management:

- Velocity calculation algorithms and sprint planning optimization are core and deserve custom modeling
- Backlog prioritization frameworks (RICE, MoSCoW) are supporting and may use established patterns
- User authentication and email notifications are generic and should use standard libraries

## Agile Domain Decomposition

Strategic design guides the agile management system into five bounded contexts:

- **Backlog Management Context** -- Product backlog ownership, user story writing, epic decomposition, refinement, and prioritization
- **Sprint Execution Context** -- Sprint lifecycle management, daily standups, sprint reviews, burndown tracking, and impediment management
- **Team & Capacity Context** -- Team formation, velocity tracking, capacity planning, skill matrices, and resource allocation
- **Continuous Improvement Context** -- Retrospective facilitation, action item tracking, metrics analysis, and process evolution
- **Release Management Context** -- Release planning, deployment coordination, feature flags, versioning, and CI/CD integration

Each context has its own model, language, and team ownership, reducing cognitive load and enabling independent evolution.

## Applying Strategic Design Iteratively

Consistent with agile principles, strategic design itself should be applied iteratively:

1. **Start with Event Storming** to discover domain events and commands across the full agile workflow
2. **Identify natural boundaries** where language and models diverge
3. **Define bounded contexts** around those boundaries
4. **Map relationships** between contexts (customer-supplier, shared kernel, etc.)
5. **Revisit and refine** as teams learn more about the domain through sprint retrospectives

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- The primary output of strategic design, defining the five agile bounded contexts
- [Context Map](../context-map/) -- Visualizes relationships between the agile bounded contexts
- [Subdomains](../subdomains/) -- Classification of agile domain areas by strategic importance
- [Ubiquitous Language](../ubiquitous-language/) -- Shared agile vocabulary within each bounded context
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects context boundaries from external tool leakage (Jira, Azure DevOps, etc.)

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapters 14-16: Strategic Design. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Part I: Strategic Design. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
