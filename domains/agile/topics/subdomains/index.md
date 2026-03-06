# Subdomains in Agile Management

## Overview

Subdomain classification is a strategic design activity that categorizes parts of the domain by their strategic importance to the business. DDD identifies three types of subdomains: Core (the competitive differentiator), Supporting (necessary but not differentiating), and Generic (commoditized, available off-the-shelf). This classification drives investment decisions, team allocation, and build-vs-buy choices. In agile management, subdomain analysis determines where to invest custom modeling effort and where to adopt existing solutions.

## Relevance to Agile Management

An agile management system encompasses dozens of capabilities, from sophisticated sprint optimization algorithms to basic user login. Treating all capabilities as equally important leads to over-engineering commodity features while under-investing in differentiating ones. Subdomain classification ensures that the most valuable engineering effort goes to the capabilities that provide the greatest competitive advantage or organizational impact.

## Core Subdomains

Core subdomains are the primary differentiators of the agile management system. They provide unique value that cannot be easily replicated by competitors or off-the-shelf tools. These deserve the best engineering talent, the most rigorous modeling, and custom development.

### Sprint Optimization

- **Description**: Algorithms and models for optimizing sprint planning, including story selection based on team capacity, skill requirements, story dependencies, and strategic priority
- **Why Core**: Effective sprint planning directly impacts team productivity and delivery predictability. The ability to recommend optimal sprint compositions based on historical data, team skills, and business priority is a key differentiator
- **Key Capabilities**: Intelligent story selection, dependency-aware planning, capacity-optimal commitment, sprint goal alignment scoring
- **Investment Level**: High -- custom algorithms, machine learning potential, deep domain modeling

### Velocity Prediction

- **Description**: Statistical models for predicting future team velocity based on historical performance, team composition changes, planned absences, and environmental factors
- **Why Core**: Accurate velocity prediction enables reliable delivery forecasting, which is one of the most valued capabilities in agile management
- **Key Capabilities**: Weighted velocity averaging, trend analysis, confidence intervals, team composition impact modeling, seasonal adjustment
- **Investment Level**: High -- custom statistical models, data science integration

### Capacity Planning Intelligence

- **Description**: Advanced capacity planning that accounts for cross-team dependencies, skill bottlenecks, planned absences, and organizational priorities
- **Why Core**: Multi-team, multi-sprint capacity planning is complex and highly organization-specific. Generic tools provide basic availability tracking but lack the intelligence to optimize across teams and time horizons
- **Key Capabilities**: Multi-sprint forecasting, skill-based allocation, cross-team dependency resolution, scenario planning ("what-if" analysis)
- **Investment Level**: High -- custom modeling, optimization algorithms

## Supporting Subdomains

Supporting subdomains are necessary for the system to function but do not provide a competitive differentiator. They may use established patterns and frameworks, and may justify moderate custom development.

### Backlog Management

- **Description**: Product backlog creation, prioritization, refinement, and story management
- **Why Supporting**: Backlog management is essential but well-understood. Established patterns exist for story writing, prioritization frameworks (RICE, MoSCoW, WSJF), and refinement workflows
- **Key Capabilities**: Story CRUD, epic decomposition, prioritization scoring, refinement tracking, dependency mapping
- **Investment Level**: Moderate -- leverage existing prioritization frameworks, customize for organizational needs

### Release Planning

- **Description**: Release scheduling, feature flag management, deployment coordination, and versioning
- **Why Supporting**: Release planning supports the delivery of value but is largely process-driven. Standard CI/CD patterns and feature flag frameworks cover most needs
- **Key Capabilities**: Release scheduling, feature flag toggles, deployment pipeline integration, release note generation, semantic versioning
- **Investment Level**: Moderate -- integrate with existing CI/CD tools, customize coordination workflows

### Retrospective Facilitation

- **Description**: Retrospective ceremony management, action item tracking, and improvement workflow
- **Why Supporting**: Retrospectives are critical for continuous improvement but follow well-established facilitation patterns. The real value is in the insights, not the tooling
- **Key Capabilities**: Retrospective templates, action item CRUD, follow-up tracking, improvement trend analysis
- **Investment Level**: Moderate -- standard CRUD with some custom analytics

### Impediment Management

- **Description**: Identification, tracking, escalation, and resolution of impediments that block sprint progress
- **Why Supporting**: Impediment tracking is necessary but follows standard issue-tracking patterns
- **Key Capabilities**: Impediment creation, assignment, escalation rules, resolution tracking, impact analysis
- **Investment Level**: Low-to-moderate -- standard workflow patterns

### Metrics and Reporting

- **Description**: Calculation and visualization of agile metrics including cycle time, lead time, throughput, burndown, burnup, and cumulative flow diagrams
- **Why Supporting**: Metrics are important for decision-making but follow well-defined formulas. Standard reporting frameworks and visualization libraries cover most needs
- **Key Capabilities**: Metric calculation engines, dashboard generation, trend analysis, exportable reports
- **Investment Level**: Moderate -- standard calculations with custom visualization and reporting

## Generic Subdomains

Generic subdomains are commoditized capabilities that are not specific to agile management. They should be acquired off-the-shelf or built using standard libraries rather than custom-developed.

### Authentication and Authorization

- **Description**: User login, role-based access control, single sign-on, multi-factor authentication
- **Why Generic**: Authentication is a solved problem with mature libraries (OAuth2, SAML, OpenID Connect). No competitive advantage comes from custom authentication
- **Investment Level**: Minimal -- use standard identity providers (Auth0, Okta, Azure AD)

### Notifications

- **Description**: Email, Slack, Teams, and push notifications for events like sprint starts, story assignments, and blocker escalations
- **Why Generic**: Notification delivery is a commodity. Standard messaging frameworks and notification services cover all requirements
- **Investment Level**: Minimal -- use standard notification services (SendGrid, SNS, Twilio)

### Audit Logging

- **Description**: Recording all significant actions for compliance, debugging, and historical analysis
- **Why Generic**: Audit logging follows standard patterns regardless of the domain. Structured logging frameworks and log aggregation services are widely available
- **Investment Level**: Minimal -- use standard logging frameworks (ELK stack, Datadog, Splunk)

### User Management

- **Description**: User profile management, team membership administration, preference settings
- **Why Generic**: Standard CRUD operations with well-established patterns
- **Investment Level**: Minimal -- standard patterns with off-the-shelf components

### File Storage

- **Description**: Attachment storage for sprint artifacts, retrospective notes, and release documentation
- **Why Generic**: Object storage is a commodity cloud service
- **Investment Level**: Minimal -- use cloud storage services (S3, Azure Blob, GCS)

## Subdomain-to-Context Mapping

| Subdomain | Type | Primary Bounded Context |
|-----------|------|------------------------|
| Sprint Optimization | Core | Sprint Execution Context |
| Velocity Prediction | Core | Team & Capacity Context |
| Capacity Planning Intelligence | Core | Team & Capacity Context |
| Backlog Management | Supporting | Backlog Management Context |
| Release Planning | Supporting | Release Management Context |
| Retrospective Facilitation | Supporting | Continuous Improvement Context |
| Impediment Management | Supporting | Sprint Execution Context |
| Metrics and Reporting | Supporting | Continuous Improvement Context |
| Authentication | Generic | Cross-cutting |
| Notifications | Generic | Cross-cutting |
| Audit Logging | Generic | Cross-cutting |
| User Management | Generic | Cross-cutting |
| File Storage | Generic | Cross-cutting |

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Subdomain classification is a key strategic design activity
- [Bounded Contexts](../bounded-contexts/) -- Subdomains map to bounded contexts, though the mapping is not always one-to-one
- [Domain Services](../domain-services/) -- Core subdomain logic is often implemented as domain services
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Generic subdomains often require ACLs to integrate with external systems
- [Integration Patterns](../integration-patterns/) -- Generic subdomains are typically integrated rather than custom-built

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 15: Distillation. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 2: Strategic Design with Subdomains. Addison-Wesley, 2016.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Cohn, Mike. "Agile Estimating and Planning." Chapters 16-18: Estimation and Planning. Prentice Hall, 2005.
