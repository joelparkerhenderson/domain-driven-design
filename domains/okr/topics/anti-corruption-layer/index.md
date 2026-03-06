# Anti-Corruption Layer

## Overview

An Anti-Corruption Layer (ACL) is a Domain-Driven Design pattern that provides a translation boundary between two systems or contexts with different models. It prevents the concepts, terminology, and assumptions of an external system from "corrupting" the internal domain model. In the OKR domain, ACLs are essential at the boundaries where the OKR system integrates with HR systems, project management tools, business intelligence platforms, and other enterprise applications.

Without ACLs, the OKR domain model would be forced to conform to the data structures and semantics of external systems, leading to a model that no longer reflects OKR domain concepts clearly.

## Why ACLs Are Necessary for OKR Systems

OKR platforms rarely exist in isolation. They operate within an enterprise ecosystem that includes:

- **HR/People Systems** (Workday, BambooHR, SAP SuccessFactors): Provide organizational hierarchy, employee data, and performance management integration.
- **Project Management Tools** (Jira, Asana, Monday.com, Linear): Track initiatives, tasks, and deliverables that contribute to key results.
- **Business Intelligence Platforms** (Tableau, Power BI, Looker): Consume OKR data for executive dashboards and strategic reporting.
- **Strategic Planning Tools** (Cascade, Quantive): May manage higher-level strategy that feeds into OKR objective setting.
- **Communication Tools** (Slack, Microsoft Teams): Deliver notifications and enable check-in workflows.
- **Identity Providers** (Okta, Azure AD): Handle authentication and user identity.

Each external system has its own data model, terminology, and assumptions. An HR system models "employees" and "departments"; an OKR system models "owners" and "teams." A project management tool tracks "epics" and "stories"; an OKR system tracks "initiatives" and "key results." The ACL translates between these worldviews.

## ACL for HR Systems

### Translation Challenges

HR systems use concepts such as:
- Employee, Manager, Department, Cost Center, Job Title, Performance Review, Competency.

The OKR domain uses:
- Owner, Contributor, Team, Organizational Level, Objective, Key Result, Score.

Key translation challenges:
- An HR "employee" maps to an OKR "owner" or "contributor," but the OKR system needs only a subset of employee data (name, ID, team membership, reporting chain) and must not import HR-specific concepts like salary bands or benefits enrollment.
- An HR "department" maps to an OKR "team," but the OKR system may define teams differently (cross-functional, project-based) than the HR organizational chart.
- HR "performance reviews" are distinct from OKR "scores." The ACL must prevent performance review concepts from contaminating the OKR scoring model, even when the two are used together in practice.

### ACL Design

```
HR System (External)          ACL                    OKR Domain (Internal)
─────────────────────    ──────────────────    ─────────────────────────
Employee                 → HREmployeeAdapter  → Owner / Contributor
Department               → HRTeamAdapter      → Team
Org Chart                → HRHierarchyAdapter → AlignmentTree (org levels)
Manager Relationship     → HRReportingAdapter → Parent-child owner mapping
Performance Review       → BLOCKED            → (not imported)
```

**HREmployeeAdapter**: Transforms an HR employee record into an OKR Owner value object containing only: PersonId, DisplayName, Email, TeamMemberships, OrganizationalLevel.

**HRTeamAdapter**: Transforms an HR department or team into an OKR Team entity. Handles the mismatch where HR teams are hierarchical by department and OKR teams may be cross-functional.

**HRHierarchyAdapter**: Translates the HR organizational chart into the organizational levels used by the Alignment & Cascading Context for cascading rules. Maps "CEO > VP > Director > Manager > IC" to abstract organizational levels.

**Sync Strategy**: The ACL periodically syncs with the HR system (daily or event-driven via HR webhook) to detect new hires, terminations, team changes, and reorganizations. Changes are translated into OKR domain events (OwnerAdded, OwnerDeactivated, TeamRestructured).

## ACL for Project Management Tools

### Translation Challenges

Project management tools use concepts such as:
- Epic, Story, Task, Sprint, Backlog, Assignee, Story Points, Status (To Do, In Progress, Done).

The OKR domain uses:
- Initiative, Key Result, Progress Update, Owner, Score, Confidence Level.

Key translation challenges:
- A project management "epic" may map to an OKR "initiative" that contributes to a key result, but the relationship is not one-to-one. Multiple epics may contribute to one key result, and an epic may span multiple key results.
- "Story points completed" may serve as automated progress data for a key result, but the OKR system should not internalize the concept of story points. The ACL translates story points into a percentage or absolute progress value.
- Project management "status" (To Do, In Progress, Done) does not directly map to OKR "confidence level" (High, Medium, Low). The ACL may derive confidence heuristically from task completion rates.

### ACL Design

```
PM Tool (External)            ACL                      OKR Domain (Internal)
──────────────────────   ─────────────────────    ──────────────────────────
Epic                     → PMInitiativeAdapter   → Initiative
Story/Task completion %  → PMProgressAdapter     → ProgressUpdate
Sprint velocity          → PMMetricsAdapter      → KPI data feed
Assignee                 → PMOwnerMapper         → Contributor reference
Project                  → PMProjectMapper       → (contextual metadata)
```

**PMInitiativeAdapter**: Maps a project management epic or project to an OKR Initiative, capturing only: InitiativeId, Name, LinkedKeyResultIds, CompletionPercentage.

**PMProgressAdapter**: Translates task completion metrics (e.g., "15 of 20 stories done") into an OKR ProgressUpdate with a normalized value. Handles different measurement types (story count, story points, custom metrics).

**PMMetricsAdapter**: Transforms project management velocity and throughput data into KPI data that the Key Result Tracking Context can use for automated key result updates.

**Sync Strategy**: Event-driven via webhooks (e.g., Jira webhooks, Asana events). When a task is completed in the PM tool, the ACL translates the event into a ProgressUpdate and publishes it to the Key Result Tracking Context.

## ACL for Business Intelligence Platforms

### Translation Challenges

BI platforms expect data in tabular, queryable formats optimized for analytics. The OKR domain model is structured around aggregates and domain events. The ACL must translate the rich domain model into flat, denormalized structures suitable for BI consumption.

### ACL Design

```
OKR Domain (Internal)         ACL                     BI Platform (External)
─────────────────────    ──────────────────────   ──────────────────────────
Objective Aggregate      → BIObjectiveExporter   → objectives_fact table
KeyResult + Score        → BIScoreExporter       → scores_fact table
AlignmentTree            → BIAlignmentExporter   → alignment_dimension table
OKRCycle                 → BICycleExporter       → cycles_dimension table
Domain Events            → BIEventStreamAdapter  → event_stream table
```

**BIObjectiveExporter**: Flattens the Objective Aggregate into a denormalized row containing objective attributes, owner information, cycle reference, and current status.

**BIScoreExporter**: Exports key result scores with full context (key result description, objective reference, cycle, scoring date) for trend analysis.

**Direction**: This ACL operates in the opposite direction from the HR and PM ACLs. Here, the OKR domain is upstream and the BI platform is downstream. The ACL ensures that the BI platform's needs for flat tables do not influence the OKR domain model's aggregate structure.

## ACL for Strategic Planning Tools

### Translation Challenges

Strategic planning tools manage concepts such as:
- Vision, Mission, Strategic Theme, Strategic Pillar, Goal, KPI.

The OKR domain manages:
- Objective, Key Result, OKR Cycle, Alignment.

The ACL translates "strategic themes" into top-level OKR objectives and "strategic KPIs" into key result targets or health metrics.

### ACL Design

**StrategyObjectiveAdapter**: Maps a strategic planning tool's "strategic goal" to a top-level OKR Objective. Preserves the strategic context as metadata on the objective without importing the planning tool's full model.

**StrategyKPIAdapter**: Translates strategic KPI definitions into key result baselines and targets, or into Health Metrics in the Analytics & Reporting Context.

## General ACL Design Principles

1. **Translate, Do Not Conform**: The ACL translates external concepts into domain concepts. The domain model never changes to accommodate an external system's data structure.

2. **Minimize Surface Area**: The ACL exposes only the data elements needed by the domain. An HR integration that provides 50 employee fields should translate to an Owner value object with 5 fields.

3. **Fail Gracefully**: When external systems are unavailable, the ACL should degrade gracefully. OKR operations should not fail because the HR system is down.

4. **Version Tolerance**: External systems change their APIs. The ACL absorbs these changes, presenting a stable interface to the domain model.

5. **Audit and Logging**: All translations should be logged for debugging and compliance. When a key result's progress is updated via PM tool integration, the audit trail should record the source data and the translated result.

6. **Idempotency**: ACL operations should be idempotent. Receiving the same webhook twice from Jira should not create duplicate progress updates.

## Relationships to Other Topics

- **Strategic Design**: ACL identification is part of Strategic Design, described in [Strategic Design](../strategic-design/).
- **Context Map**: ACLs appear on the context map where bounded contexts interface with external systems, documented in [Context Map](../context-map/).
- **Bounded Contexts**: ACLs protect bounded context model integrity, as defined in [Bounded Contexts](../bounded-contexts/).
- **Subdomains**: Generic subdomains (authentication, notifications) are wrapped behind ACLs, as classified in [Subdomains](../subdomains/).
- **Integration Patterns**: ACLs are a key mechanism within broader integration patterns, detailed in [Integration Patterns](../integration-patterns/).
- **Domain Events**: ACLs translate external events into domain events, documented in [Domain Events](../domain-events/).
- **Regulatory Compliance**: ACLs play a role in data governance and audit trails, covered in [Regulatory Compliance](../regulatory-compliance/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity (Anticorruption Layer).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 3: Context Maps (Anticorruption Layer pattern).
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
- Hohpe, Gregor and Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
