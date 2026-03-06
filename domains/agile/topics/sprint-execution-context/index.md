# Sprint Execution Context in Agile Management

## Overview

The Sprint Execution Context is the bounded context responsible for the full lifecycle of a sprint -- from planning through daily execution to review and completion. This context manages the sprint backlog, daily standups, burndown tracking, impediment management, and sprint goal achievement. It represents the delivery team's operational domain: the day-to-day work of turning backlog items into completed increments within a fixed time-box.

## Relevance to Agile Management

The sprint is the heartbeat of Scrum-based agile development. Every one to four weeks, a team commits to a sprint goal, works through a prioritized sprint backlog, tracks progress via burndown charts and daily standups, addresses blockers, and delivers a potentially shippable increment. The Sprint Execution Context provides the structure and rules that keep this cycle disciplined, transparent, and sustainable.

## Core Responsibilities

### Sprint Planning

Sprint planning is a collaborative ceremony where the team selects items from the refined product backlog and commits to a sprint goal. The team pulls items in priority order until capacity is consumed. Planning produces two artifacts: the sprint goal (a concise statement of what the sprint will achieve) and the sprint backlog (the set of items the team commits to completing).

### Daily Standup

The daily standup (or daily scrum) is a time-boxed synchronization event, typically 15 minutes. Each team member communicates what they completed since the last standup, what they plan to work on next, and any impediments blocking progress. The standup is for coordination, not status reporting -- it enables the team to adapt the plan for the day.

### Burndown Tracking

The burndown chart visualizes remaining work (typically in story points or task hours) against time. It provides early warning when the team is off-pace to complete the sprint commitment. Burndown snapshots are recorded daily, enabling trend analysis and forecasting.

### Impediment Management

Impediments are obstacles that prevent the team from making progress. They range from technical blockers (environment unavailability, dependency failures) to organizational issues (unclear requirements, competing priorities). The Scrum Master is responsible for tracking impediments and facilitating their resolution. Unresolved impediments escalate and may compromise the sprint goal.

### Sprint Review

The sprint review is a ceremony at the end of the sprint where the team demonstrates completed work to stakeholders. It provides feedback that informs backlog refinement and future sprint planning. Only items meeting the Definition of Done are presented.

## Aggregate Roots

- **Sprint**: The primary aggregate root, containing the sprint backlog, burndown snapshots, impediments, and the sprint goal

## Key Invariants

- A sprint must have a sprint goal before transitioning to Active state
- Sprint duration (time-box) is fixed once the sprint starts and cannot be changed
- Items cannot be added to an active sprint without explicit team agreement (recorded as a SprintScopeChange event)
- Only items meeting the Definition of Done can be marked as completed
- Total committed story points must not exceed team capacity
- Only one sprint per team can be in Active state at any time

## Sprint Lifecycle

The Sprint entity follows a defined state machine:

- **Draft**: Sprint is created but not yet planned. No items committed.
- **Planned**: Sprint planning is complete. Sprint goal and sprint backlog are defined. Team has committed.
- **Active**: Sprint is in progress. Daily standups occur. Burndown is tracked. Impediments are managed.
- **InReview**: Sprint review is conducted. Completed items are demonstrated. Stakeholder feedback is collected.
- **Completed**: Sprint is closed. Completion metrics are finalized. SprintCompleted event is published.
- **Cancelled**: (Exception path) Sprint is terminated early. Completed items are returned to the product backlog for re-prioritization.

## Domain Events Produced

- SprintPlanned -- Sprint planning concludes with a committed sprint backlog
- SprintStarted -- Sprint transitions from Planned to Active
- DailyStandupRecorded -- A daily standup summary is captured
- StoryCompleted -- A sprint backlog item meets the Definition of Done
- StoryBlocked -- An impediment is raised against a story
- SprintScopeChanged -- Items are added to or removed from an active sprint
- SprintCompleted -- Sprint transitions to Completed state

## Domain Events Consumed

- BacklogRefined (from Backlog Management) -- Updates the pool of ready items available for sprint planning
- VelocityCalculated (from Team & Capacity) -- Informs the planning baseline for story point commitment
- RetroCompleted (from Continuous Improvement) -- Incorporates action items into the next sprint

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Sprint Execution is one of the five agile bounded contexts
- [Backlog Management Context](../backlog-management-context/) -- Supplies the refined, prioritized items that feed sprint planning
- [Team & Capacity Context](../team-capacity-context/) -- Provides velocity and capacity data for sprint commitment decisions
- [Continuous Improvement Context](../continuous-improvement-context/) -- Receives sprint completion data for retrospective analysis
- [Aggregates](../aggregates/) -- Sprint is the aggregate root with SprintBacklogItem as an internal entity
- [Value Objects](../value-objects/) -- SprintGoal, TimeBox, BurndownSnapshot, and DefinitionOfDone are value objects in this context

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." Sections: Sprint, Sprint Planning, Daily Scrum, Sprint Review. 2020.
- Cohn, Mike. "Agile Estimating and Planning." Chapters 14-18: Iteration Planning and Tracking. Prentice Hall, 2005.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
