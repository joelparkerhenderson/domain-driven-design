# Anti-Corruption Layer in Agile Management

## Overview

An anti-corruption layer (ACL) is a translation boundary that prevents an external system's model from leaking into and corrupting a bounded context's internal domain model. The ACL translates between the external system's concepts, data structures, and terminology and the bounded context's ubiquitous language. In agile management, ACLs are essential for integrating with external tools like Jira, Azure DevOps, GitHub Issues, CI/CD pipelines, and communication platforms while maintaining a clean internal domain model.

## Relevance to Agile Management

Agile teams rarely operate in isolation from external tooling. Most organizations use one or more of the following:

- **Issue trackers**: Jira, Azure DevOps, GitHub Issues, Linear, Shortcut, Rally
- **CI/CD systems**: Jenkins, GitHub Actions, GitLab CI, CircleCI, Azure Pipelines
- **Communication platforms**: Slack, Microsoft Teams, Discord
- **Time tracking**: Toggl, Harvest, Clockify
- **Documentation**: Confluence, Notion, Google Docs

Each of these systems has its own data model, terminology, and assumptions. Without ACLs, the agile domain model becomes polluted with external system concepts -- Jira's "issue types," Azure DevOps's "work item states," GitHub's "labels" -- making the system brittle and tightly coupled to specific vendor implementations.

## ACL Architecture

An anti-corruption layer typically consists of three components:

### Facade

The facade provides a simplified interface to the external system, hiding its complexity from the domain layer. It exposes only the operations the domain needs.

### Adapter

The adapter translates between the external system's data structures and the domain model's entities and value objects. It maps foreign concepts to domain concepts.

### Translator

The translator converts data formats, maps enumeration values, and handles semantic differences between systems.

## ACLs for Specific External Systems

### Jira Integration ACL

Jira has a rich but opinionated data model that differs significantly from a clean agile domain model.

**Key Translations**:

| Jira Concept | Domain Concept | Translation Notes |
|-------------|---------------|-------------------|
| Issue (type: Story) | UserStory | Jira's "issue" is generic; domain distinguishes stories, tasks, and bugs explicitly |
| Issue (type: Epic) | Epic | Direct mapping, but Jira epics have custom fields that need filtering |
| Sprint | Sprint | Near-direct mapping, but Jira sprints lack Sprint Goal as a first-class concept |
| Story Points (custom field) | StoryPoint (value object) | Jira stores as a generic custom field; domain models as a typed value object |
| Status (To Do/In Progress/Done) | WorkflowState | Jira allows custom statuses; ACL must map to domain-defined states |
| Priority (Highest/High/Medium/Low/Lowest) | Priority (MoSCoW) | Different prioritization models require explicit mapping |
| Component | Not directly mapped | Jira components may map to team or feature area concepts |
| Label | Tag (value object) | Flexible mapping, but domain may restrict allowed tag values |

**Invariant Protection**:
- Jira allows stories without acceptance criteria; the ACL enforces that imported stories must have acceptance criteria or are flagged as "not ready"
- Jira allows sprint scope changes at any time; the ACL enforces sprint commitment rules
- Jira's custom workflow may not align with domain states; the ACL maps and validates

### Azure DevOps Integration ACL

Azure DevOps uses a different terminology and structure than the domain model.

**Key Translations**:

| Azure DevOps Concept | Domain Concept | Translation Notes |
|---------------------|---------------|-------------------|
| Work Item (type: User Story) | UserStory | Azure DevOps work items are more structured than Jira issues |
| Work Item (type: Epic) | Epic | Direct mapping with field translation |
| Iteration | Sprint | Azure DevOps uses "iteration" terminology; ACL maps to Sprint |
| Effort (field) | StoryPoint | Different field name, same concept |
| Area Path | Team/Feature Area | Hierarchical; requires flattening to domain concepts |
| State (New/Active/Resolved/Closed) | WorkflowState | Different state names require mapping |
| Board Column | KanbanColumn | Azure DevOps board columns map to workflow states |
| Query | Not directly mapped | Azure DevOps queries are a UI concept, not a domain concept |

**Invariant Protection**:
- Azure DevOps allows arbitrary state transitions; the ACL enforces valid workflow state transitions
- Area paths may not map cleanly to team boundaries; the ACL validates team assignments

### GitHub Issues Integration ACL

GitHub Issues has a minimalist data model that requires enrichment for full agile management.

**Key Translations**:

| GitHub Concept | Domain Concept | Translation Notes |
|---------------|---------------|-------------------|
| Issue | UserStory or Task | GitHub does not distinguish; the ACL uses labels or templates to classify |
| Milestone | Sprint or Release | Overloaded concept; ACL must determine which based on naming convention |
| Label | Priority, Type, Status | GitHub uses labels for everything; ACL must parse and categorize |
| Project Board Column | WorkflowState | GitHub project boards provide Kanban-style tracking |
| Assignee | TeamMember | Direct mapping with identity translation |

**Invariant Protection**:
- GitHub Issues lack native story point fields; the ACL may use label conventions (e.g., "points:3") or custom fields
- GitHub milestones do not enforce time-boxing; the ACL adds sprint start/end date validation

### CI/CD System ACL

CI/CD systems (Jenkins, GitHub Actions, GitLab CI, etc.) operate in the deployment domain but interact with the Release Management Context.

**Key Translations**:

| CI/CD Concept | Domain Concept | Translation Notes |
|--------------|---------------|-------------------|
| Pipeline Run | DeploymentExecution | CI/CD pipelines become deployment events in the domain |
| Build Status (pass/fail) | DeploymentResult | Binary outcome mapped to domain result states |
| Environment (staging/production) | DeploymentTarget | CI/CD environments map to domain deployment targets |
| Artifact | ReleaseArtifact | Build output becomes a release artifact |
| Pipeline Stage | Not directly mapped | Internal CI/CD stages are implementation details |

**Invariant Protection**:
- CI/CD systems may deploy without domain authorization; the ACL enforces go/no-go decisions
- Deployment to production requires all quality gates to pass; the ACL validates before triggering

### Communication Platform ACL (Slack/Teams)

Communication platforms integrate for notifications and ceremony coordination.

**Key Translations**:

| Platform Concept | Domain Concept | Translation Notes |
|-----------------|---------------|-------------------|
| Channel | Team or Context | Slack/Teams channels map to team communication spaces |
| Message | Notification | Platform messages become domain notifications |
| Scheduled Message | CeremonyReminder | Scheduled messages for standup/retro reminders |
| Reaction/Emoji | Acknowledgment | Quick reactions map to domain acknowledgments (e.g., standup check-in) |

## ACL Implementation Patterns

### Synchronization Strategies

- **Event-driven sync**: External system webhooks trigger ACL translation and domain event publication
- **Polling sync**: ACL periodically polls external systems for changes (fallback when webhooks are unavailable)
- **Bidirectional sync**: Changes in the domain model are translated back to the external system through the ACL
- **One-way import**: External system is the source of truth; domain model is read-only for those concepts

### Conflict Resolution

When the same data exists in both the domain model and an external system:

- **External system wins**: For data owned by the external system (e.g., CI/CD pipeline status)
- **Domain model wins**: For data governed by domain invariants (e.g., sprint commitment rules)
- **Last-write wins**: For non-critical data where eventual consistency is acceptable
- **Manual resolution**: For conflicts that require human judgment (e.g., conflicting priority assignments)

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- ACLs protect bounded context boundaries from external system leakage
- [Context Map](../context-map/) -- ACLs are placed at integration points identified in the context map
- [Integration Patterns](../integration-patterns/) -- ACLs implement specific integration patterns for each external system
- [Ubiquitous Language](../ubiquitous-language/) -- ACLs translate between external terminology and the bounded context's ubiquitous language
- [Domain Events](../domain-events/) -- External system changes are translated into domain events by the ACL
- [Repositories](../repositories/) -- ACLs may implement repository interfaces that wrap external system APIs

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity -- Anti-Corruption Layer. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps -- Anti-Corruption Layer. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Hohpe, Gregor and Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
