# Continuous Improvement Context in Agile Management

## Overview

The Continuous Improvement Context is the bounded context responsible for retrospective facilitation, action item tracking, agile metrics analysis, and process evolution. This context embodies the Agile Manifesto principle: "At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly." It captures what is learned from each sprint cycle and transforms those insights into concrete process improvements, measured by flow metrics such as cycle time, lead time, and throughput.

## Relevance to Agile Management

Agile without continuous improvement is process theater -- teams go through the motions of sprints and standups without actually getting better. The Continuous Improvement Context provides the structured feedback loop that distinguishes high-performing agile teams from teams that merely follow ceremonies. By tracking metrics over time and tying retrospective action items to measurable outcomes, this context ensures that process changes are evidence-based rather than anecdotal.

## Core Responsibilities

### Retrospectives

Retrospectives are structured reflection sessions held after each sprint. The team examines what went well, what could improve, and what new ideas to try. Common facilitation formats include:

- **Start/Stop/Continue**: Identify behaviors to begin, cease, or maintain
- **4Ls (Liked, Learned, Lacked, Longed For)**: Categorize observations by emotional response
- **Mad/Sad/Glad**: Emotional temperature check of team sentiment
- **Sailboat**: Visual metaphor -- wind (accelerators), anchors (impediments), rocks (risks), destination (goals)
- **Timeline**: Chronological review of sprint events to identify patterns

Every retrospective must produce at least one action item. Action items without owners and deadlines are wishes, not commitments.

### Action Items

Action items are the concrete outputs of retrospectives. Each action item must have an assignee (individual responsible for completion), a target date (deadline for completion), and a measurable definition of done. Action items are tracked across sprints -- their completion status is reviewed at the beginning of each subsequent retrospective to ensure accountability and follow-through.

### Metrics

**Cycle Time**: The time from when work begins on an item (InProgress) to when it is completed (Done). Cycle time measures the team's execution speed. Lower cycle time indicates faster delivery. Teams track cycle time distributions (mean, median, 85th percentile) rather than individual data points.

**Lead Time**: The time from when an item is requested (enters the backlog) to when it is delivered (deployed to production). Lead time measures the total time a customer waits for a feature. It encompasses both queue time (waiting in the backlog) and cycle time (active work).

**Throughput**: The number of items completed per unit of time (typically per sprint or per week). Throughput measures the team's delivery rate. Unlike velocity (which measures story points), throughput measures item count, making it framework-agnostic.

**Cumulative Flow Diagram (CFD)**: A visualization showing the number of items in each workflow state over time. CFDs reveal bottlenecks (where work accumulates), flow efficiency, and WIP trends.

### Kaizen

Kaizen is the Japanese philosophy of continuous incremental improvement. In the agile context, Kaizen means that every sprint should be slightly better than the last. Rather than pursuing large-scale process overhauls, teams make small, safe-to-fail experiments: reducing WIP limits by one, adding a checklist item to the Definition of Done, pairing on code reviews for a sprint. Each experiment is measured against a hypothesis, and the results inform the next iteration.

## Aggregate Roots

- **Retrospective**: The primary aggregate root, containing observations, action items, and the facilitation format used

## Key Invariants

- Every retrospective must be associated with a completed sprint
- Every retrospective must produce at least one action item before it can be closed
- Action items must have an assignee and a target completion date
- Metrics are calculated from historical data and cannot be manually overridden
- A retrospective cannot be reopened once closed

## Domain Events Produced

- RetroStarted -- A retrospective session begins
- ObservationRecorded -- A team member contributes an observation
- ActionItemCreated -- An action item is identified and assigned
- RetroCompleted -- The retrospective session concludes
- ActionItemCompleted -- An action item from a previous retrospective is finished
- MetricThresholdBreached -- A monitored metric crosses a defined alerting threshold

## Domain Events Consumed

- SprintCompleted (from Sprint Execution) -- Triggers retrospective scheduling and provides sprint data for metric calculation
- StoryCompleted (from Sprint Execution) -- Provides cycle time data points
- StoryBlocked (from Sprint Execution) -- Tracks impediment frequency for trend analysis
- ReleaseDeployed (from Release Management) -- Provides lead time endpoints for delivered stories

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Continuous Improvement is one of the five agile bounded contexts
- [Sprint Execution Context](../sprint-execution-context/) -- Provides the sprint data that feeds retrospective analysis and metric calculation
- [Team & Capacity Context](../team-capacity-context/) -- Receives team health indicators from retrospective outcomes
- [Aggregates](../aggregates/) -- Retrospective is the aggregate root with ActionItem as an internal entity
- [Value Objects](../value-objects/) -- CycleTime, LeadTime, and Observation are value objects in this context
- [Domain Events](../domain-events/) -- RetroCompleted and ActionItemCompleted communicate outcomes to other contexts

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." Section: Sprint Retrospective. 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Chapters on Flow Metrics and Continuous Improvement. Blue Hole Press, 2010.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
- Imai, Masaaki. "Kaizen: The Key to Japan's Competitive Success." McGraw-Hill, 1986.
