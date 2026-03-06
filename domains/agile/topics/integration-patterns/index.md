# Integration Patterns in Agile Management

## Overview

Integration patterns define how agile bounded contexts communicate with each other and with external systems. In Domain-Driven Design, integration patterns include shared kernel, published language, open host service, customer-supplier, and anti-corruption layer. In agile management, these patterns govern how the domain model interacts with project management tools (Jira, Azure DevOps, GitHub), communication platforms (Slack, Microsoft Teams), and CI/CD pipelines -- while preserving the integrity of the domain model.

## Relevance to Agile Management

No agile team operates in isolation from tooling. Backlogs live in Jira or Azure DevOps, code lives in GitHub or GitLab, builds run in Jenkins or GitHub Actions, and notifications flow through Slack or Teams. Integration patterns ensure that these external tools serve the domain model rather than dictating it. Without deliberate integration design, the domain model degrades into a reflection of the tool's data model rather than the team's actual agile process.

## DDD Integration Patterns Applied to Agile

### Published Language

A well-documented, shared schema that defines how contexts exchange information. In agile management, the published language specifies the structure of domain events (SprintCompleted payload, StoryBlocked payload) that flow between bounded contexts. All contexts agree on the published language, enabling independent evolution of internal models while maintaining interoperability.

### Open Host Service

A context exposes a well-defined API for other contexts or external systems to consume. The Sprint Execution Context might expose an open host service that allows the Release Management Context to query completed increments, or that allows an external dashboard to display burndown data. The API is designed around domain concepts, not infrastructure details.

### Customer-Supplier

An upstream context (supplier) provides data or services that a downstream context (customer) depends on. In agile management, the Backlog Management Context is the supplier of ready items to the Sprint Execution Context (customer). The customer's needs influence the supplier's priorities -- if sprint planning consistently finds that items are not refined enough, the Backlog Management Context adjusts its refinement process.

### Shared Kernel

Two contexts agree to share a small, explicitly defined subset of the domain model. In agile management, a shared kernel might include the StoryID type, the DefinitionOfDone value object, or the SprintStatus enumeration. Changes to the shared kernel require agreement from all contexts that use it. Shared kernels should be kept small to minimize coupling.

### Conformist

A downstream context conforms to the upstream context's model without translation. This pattern is common when integrating with dominant external tools: a small team might conform to Jira's data model rather than building a translation layer, accepting that Jira's concepts (issues, sprints, boards) define their vocabulary.

## External Tool Integration

### Jira / Azure DevOps / GitHub Projects

- **Pattern**: Anti-Corruption Layer (preferred) or Conformist (pragmatic)
- **Approach**: The anti-corruption layer translates between the tool's data model (Jira issue, Azure DevOps work item) and the domain model (UserStory, BacklogItem). This prevents tool-specific concepts (Jira's "issue type," Azure DevOps's "area path") from leaking into domain logic.
- **Sync Strategy**: Event-driven synchronization via webhooks. When a story is completed in the domain, a webhook updates the external tool. When a tool event occurs (e.g., a story is moved on a Jira board), the anti-corruption layer translates it into a domain event.
- **Risk**: Without an ACL, domain concepts become subordinate to the tool's model, making tool migration extremely costly.

### CI/CD Pipelines (GitHub Actions, Jenkins, GitLab CI)

- **Pattern**: Open Host Service
- **Approach**: The Release Management Context exposes an API that CI/CD pipelines call to report build status, test results, and deployment outcomes. The pipeline does not write directly to the domain's data store -- it communicates through the context's service interface.
- **Events**: Pipeline completion triggers ReleaseDeployed or deployment failure events that the domain processes through its own business rules.

### Communication Platforms (Slack, Microsoft Teams)

- **Pattern**: Published Language with Anti-Corruption Layer
- **Approach**: Domain events (SprintStarted, StoryBlocked, ReleaseDeployed) are translated into platform-specific notification formats. The anti-corruption layer converts domain concepts into human-readable messages with actionable links.
- **Notifications**: SprintCompleted triggers a summary message with velocity and goal achievement; StoryBlocked triggers an alert to the Scrum Master channel; ReleaseDeployed triggers a release announcement with version and feature summary.

## Integration Design Guidelines

1. **Prefer anti-corruption layers over conformist**: Invest in translation layers that protect the domain model from external tool concepts, even if it requires more initial effort. Tool migrations become dramatically cheaper.

2. **Use domain events for async integration**: Synchronous API calls between contexts create temporal coupling. Prefer asynchronous event-based integration that allows contexts to process at their own pace.

3. **Version integration contracts**: External tool APIs and webhook payloads change over time. Version your integration contracts and handle multiple versions simultaneously during transitions.

4. **Keep the domain model tool-agnostic**: The domain should model agile concepts (stories, sprints, velocity), not tool concepts (Jira issues, Azure DevOps iterations). If switching tools requires rewriting domain logic, the integration boundary was drawn incorrectly.

5. **Monitor integration health**: Track webhook delivery success rates, sync latency, and data consistency between the domain and external tools. Integration failures are often silent.

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- Primary pattern for protecting domain integrity during external tool integration
- [Bounded Contexts](../bounded-contexts/) -- Integration patterns define how bounded contexts communicate
- [Event-Driven Architecture](../event-driven-architecture/) -- Provides the infrastructure for event-based integration
- [Domain Events](../domain-events/) -- Events are the primary payload exchanged through integration patterns
- [Ubiquitous Language](../ubiquitous-language/) -- Published language formalizes the ubiquitous language for cross-context communication

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity -- Context Map patterns. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
- Hohpe, Gregor and Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
