# Subdomains

## Overview

In Domain-Driven Design, subdomains represent distinct areas of business capability within a larger domain. They are classified into three types based on strategic importance: Core Subdomains (providing competitive advantage), Supporting Subdomains (necessary but not differentiating), and Generic Subdomains (commodity capabilities). This classification guides investment decisions, build-vs-buy choices, and team allocation for the OKR system.

## Subdomain Classification

### Core Subdomains

Core subdomains contain the logic that differentiates the OKR system and provides unique business value. These deserve the highest investment in modeling quality, developer talent, and architectural attention.

#### Adaptive Goal-Setting

**Description**: The intelligence that guides users in creating well-formed, strategically aligned objectives and key results. This includes SMART criteria validation, OKR type recommendation (committed vs. aspirational), and contextual suggestions based on organizational strategy and historical performance.

**Why Core**: Generic task management tools allow users to create any kind of goal. An OKR system's value lies in actively guiding users toward objectives that are ambitious yet achievable, and key results that are truly measurable. This adaptive capability is what differentiates a purpose-built OKR platform from a spreadsheet.

**Key Capabilities**:
- SMART criteria enforcement with intelligent feedback (not just validation, but suggestions for improvement).
- OKR type classification logic that recommends committed vs. aspirational based on historical scoring patterns.
- Objective quality scoring that evaluates clarity, measurability, and strategic fit.
- Template-based objective creation informed by organizational context and industry patterns.

**Bounded Context**: Primarily resides in the Objective Setting Context.

#### Scoring Algorithms

**Description**: The algorithms that translate raw key result measurements into meaningful scores, aggregate scores across key results into objective-level scores, and incorporate confidence levels, weighting, and OKR type into the final evaluation.

**Why Core**: Scoring is the mechanism by which the OKR system produces actionable insight. Naive scoring (simple percentage completion) fails to capture the nuance of aspirational goals, weighted key results, or confidence-adjusted projections. Sophisticated scoring algorithms are a core differentiator.

**Key Capabilities**:
- Key result scoring on a 0.0-to-1.0 scale with configurable formulas (linear, logarithmic, threshold-based).
- Objective-level score aggregation with key result weighting.
- Confidence-adjusted scoring that factors in likelihood of achievement.
- OKR-type-aware scoring where aspirational OKRs use different success thresholds than committed OKRs.
- Historical score normalization for cross-cycle trend analysis.

**Bounded Context**: Primarily resides in the Key Result Tracking Context with analytical extensions in the Analytics & Reporting Context.

#### Alignment Intelligence

**Description**: The logic that validates, optimizes, and visualizes the alignment between objectives across organizational levels. This includes conflict detection, gap analysis, dependency mapping, and recommendations for improving strategic coherence.

**Why Core**: Alignment is the essence of OKR practice. Without intelligent alignment, OKRs become isolated goals that fail to produce organizational coherence. The ability to detect misalignment, circular dependencies, and strategic gaps is what makes an OKR system strategically valuable.

**Key Capabilities**:
- Automatic alignment validation when objectives are created or modified.
- Circular dependency detection in alignment trees.
- Alignment gap identification (strategic objectives without supporting child objectives).
- Conflict detection where child objectives may undermine each other.
- Cross-team dependency mapping and visualization.
- Alignment health scoring at the organizational level.

**Bounded Context**: Primarily resides in the Alignment & Cascading Context.

### Supporting Subdomains

Supporting subdomains are necessary for the system to function but do not provide competitive differentiation. They may be built in-house with less rigor than core subdomains, or partially leveraged from existing solutions.

#### Cascading Mechanics

**Description**: The rules and processes that govern how high-level strategic objectives decompose into team-level and individual-level OKRs. While related to alignment intelligence (a core subdomain), the mechanical aspects of cascading -- creating parent-child relationships, enforcing hierarchy rules, propagating changes -- are supporting rather than core.

**Key Capabilities**:
- Parent-child objective linking.
- Cascade propagation (when a parent objective changes, child objectives are notified).
- Organizational hierarchy traversal for cascade views.
- Bulk cascading operations for organizational restructuring.

**Bounded Context**: Resides in the Alignment & Cascading Context.

#### Review & Cadence Management

**Description**: The management of OKR cycles, check-in schedules, retrospective ceremonies, and the temporal workflow of OKR practice. While essential, cycle management is a well-understood scheduling problem that does not require deep innovation.

**Key Capabilities**:
- Cycle lifecycle management (Planning, Active, Scoring, Closed).
- Check-in scheduling and reminders.
- Retrospective and grading ceremony coordination.
- Calendar integration for OKR events.

**Bounded Context**: Resides in the Review & Cadence Context.

#### Reporting & Dashboards

**Description**: The generation of reports, dashboards, and data visualizations from OKR data. While important for user value, reporting is a well-understood domain with many commodity tools available.

**Key Capabilities**:
- Real-time dashboards with OKR health metrics.
- Cycle-over-cycle trend reports.
- Exportable reports (PDF, CSV) for stakeholder reviews.
- Configurable dashboard layouts.

**Bounded Context**: Resides in the Analytics & Reporting Context.

#### Progress Tracking

**Description**: The mechanics of recording check-ins, updating progress, managing confidence levels, and maintaining the audit trail of key result status changes.

**Key Capabilities**:
- Manual progress entry with narrative updates.
- Automated progress ingestion from integrated data sources.
- Confidence level tracking with trend visualization.
- Progress history and audit trail.

**Bounded Context**: Resides in the Key Result Tracking Context.

### Generic Subdomains

Generic subdomains provide commodity capabilities that are common across many types of applications. These should be purchased, leveraged from existing infrastructure, or built with minimal custom code.

#### Authentication & Authorization

**Description**: User identity management, login, single sign-on (SSO), and role-based access control (RBAC). Every enterprise application needs authentication, and the OKR domain has no unique requirements.

**Capabilities**: SSO integration (SAML, OIDC), role management (Admin, OKR Coach, Manager, Individual Contributor), permission enforcement.

**Recommendation**: Use an identity provider (Okta, Auth0, Azure AD) rather than building custom.

#### Notifications & Reminders

**Description**: Email notifications, in-app notifications, and push notifications for OKR events (check-in reminders, cycle transitions, alignment requests).

**Capabilities**: Multi-channel notification delivery, user preference management, notification templates, quiet hours.

**Recommendation**: Use a notification service (SendGrid, SNS, or built-in platform notifications) rather than building custom.

#### User & Organization Management

**Description**: Managing user profiles, team structures, organizational hierarchies, and department assignments. This data feeds into the OKR system but is not OKR-specific.

**Capabilities**: User CRUD, team management, organizational hierarchy maintenance, role assignment.

**Recommendation**: Integrate with existing HR/people systems (Workday, BambooHR) rather than maintaining a separate user directory.

#### Audit Logging

**Description**: Recording all system actions for compliance and debugging purposes.

**Capabilities**: Immutable audit log, event replay, compliance reporting.

**Recommendation**: Use infrastructure-level logging (ELK stack, Datadog) with domain event capture.

## Subdomain-to-Bounded-Context Mapping

| Subdomain | Type | Primary Bounded Context |
|-----------|------|------------------------|
| Adaptive Goal-Setting | Core | Objective Setting Context |
| Scoring Algorithms | Core | Key Result Tracking Context |
| Alignment Intelligence | Core | Alignment & Cascading Context |
| Cascading Mechanics | Supporting | Alignment & Cascading Context |
| Review & Cadence Management | Supporting | Review & Cadence Context |
| Reporting & Dashboards | Supporting | Analytics & Reporting Context |
| Progress Tracking | Supporting | Key Result Tracking Context |
| Authentication & Authorization | Generic | External / Shared Infrastructure |
| Notifications & Reminders | Generic | External / Shared Infrastructure |
| User & Organization Management | Generic | External / HR Integration |
| Audit Logging | Generic | External / Shared Infrastructure |

## Investment Strategy

Based on subdomain classification:

- **Core**: Invest heavily. Assign senior developers. Use the richest DDD tactical patterns (aggregates, domain events, domain services). Write comprehensive tests. Iterate frequently based on domain expert feedback.
- **Supporting**: Invest moderately. Follow standard engineering practices. May use simpler patterns (CRUD with domain validation). Acceptable to use frameworks and libraries that reduce custom code.
- **Generic**: Minimize investment. Buy or integrate rather than build. Use off-the-shelf solutions. Wrap behind anti-corruption layers to protect core domain models.

## Relationships to Other Topics

- **Strategic Design**: Subdomain identification is a key output of Strategic Design, described in [Strategic Design](../strategic-design/).
- **Bounded Contexts**: Subdomains map to bounded contexts as documented in [Bounded Contexts](../bounded-contexts/).
- **Anti-Corruption Layer**: Generic subdomains are isolated behind ACLs as described in [Anti-Corruption Layer](../anti-corruption-layer/).
- **Domain Services**: Core subdomain logic is often implemented as domain services, detailed in [Domain Services](../domain-services/).
- **Integration Patterns**: Generic subdomain integrations are covered in [Integration Patterns](../integration-patterns/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 15: Distillation (Core Domain, Generic Subdomains).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
