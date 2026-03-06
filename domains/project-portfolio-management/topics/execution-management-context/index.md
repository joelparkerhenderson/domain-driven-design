# Execution Management Context in Project Portfolio Management

## Overview

The Execution Management Context handles the day-to-day management of project delivery: project plans, work breakdown structures, milestones, tasks, deliverables, status reporting, and change management.

## Core Responsibilities

- **Project Planning**: Creating and maintaining project plans with WBS, milestones, and task dependencies
- **Schedule Management**: Tracking progress against planned schedules, critical path analysis
- **Task Management**: Assigning, tracking, and completing individual work items
- **Deliverable Management**: Defining, producing, and accepting project deliverables
- **Status Reporting**: Generating RAG status reports for stakeholders and governance
- **Change Management**: Processing change requests that affect scope, schedule, or budget

## Aggregates

### Project Aggregate

- **Root**: Project (identified by ProjectID)
- **Internal entities**: Milestone, Task, Deliverable
- **Value objects**: DateRange, RAGStatus, WBSElement, Dependency, Progress
- **States**: Planning → Executing → Monitoring → Closing → Completed
- **Invariants**: Tasks fall within milestone dates; milestones fall within project dates; dependencies don't create cycles; completed tasks cannot be reopened without change request

### ChangeRequest Aggregate

- **Root**: ChangeRequest (identified by ChangeRequestID)
- **Value objects**: ImpactAssessment (scope, schedule, budget), ChangeCategory, Justification
- **States**: Submitted → Under Review → Approved / Rejected → Implemented
- **Invariants**: Must include impact assessment; approved changes update project baseline

## Domain Events

| Event | Consumers |
|-------|-----------|
| ProjectStarted | Resource, Financial |
| TaskCompleted | Schedule update, EVM |
| MilestoneCompleted | Governance, Financial |
| StatusReported | Governance dashboard |
| ChangeRequestApproved | Financial, Resource |
| ProjectCompleted | Governance, Financial, Resource |
| DeliverableAccepted | Milestone progress |

## Integration

- **Governance → Execution**: ProjectApproved triggers plan creation
- **Execution → Financial**: Progress events drive earned value
- **Resource → Execution**: Allocation changes affect schedules
- **Execution → Risk**: Activity progress informs risk assessment

## Key Methodologies

- **Waterfall**: Sequential phases with gate reviews between stages
- **Agile/Scrum**: Iterative sprints with backlog management
- **Hybrid**: Phase-gated with agile execution within phases
- **Kanban**: Continuous flow with WIP limits

The Execution context supports multiple methodologies through configurable project templates.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of five PPM bounded contexts
- [Aggregates](../aggregates/) — Project, ChangeRequest are aggregate roots
- [Domain Services](../domain-services/) — CriticalPathAnalyzer
- [Value Objects](../value-objects/) — RAGStatus, DateRange, WBSElement

## References

- PMI. "PMBOK Guide." 7th ed. PMI, 2021.
- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Schwaber, Ken & Sutherland, Jeff. "The Scrum Guide." 2020.
