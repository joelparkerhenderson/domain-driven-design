# Team & Capacity Context in Agile Management

## Overview

The Team & Capacity Context is the bounded context responsible for team formation and composition, velocity tracking and prediction, capacity planning, skill matrix management, and resource allocation. This context models the people side of agile delivery -- understanding who is available, what they can do, how fast the team works, and how much work can be committed in a given sprint. It provides the quantitative foundation that makes sprint planning realistic rather than aspirational.

## Relevance to Agile Management

Sustainable agile delivery depends on realistic capacity planning. Teams that consistently over-commit burn out; teams that under-commit waste potential. The Team & Capacity Context provides the data and calculations that keep commitments achievable: velocity tracks what the team has historically delivered, capacity accounts for actual availability (holidays, PTO, partial allocations), and skill matrices ensure that the right expertise is available for planned work.

## Core Responsibilities

### Team Formation

Teams are cross-functional groups organized to deliver value independently. Team formation involves selecting members with complementary skills, establishing working agreements, and defining team norms. In DDD terms, a Team is an aggregate root that manages its own membership, and each team has a lifecycle (Forming, Active, Disbanded) that reflects its organizational status.

### Velocity Tracking

Velocity measures the amount of work a team completes per sprint, typically expressed in story points. It is calculated from completed sprints only -- in-progress and cancelled sprints are excluded. Velocity serves as a planning input, not a performance metric. Common calculation methods include:

- **Simple Average**: Sum of completed points divided by number of sprints
- **Weighted Average**: Recent sprints count more heavily (e.g., last sprint weighted at 2x)
- **Rolling Average**: Sliding window of the most recent N sprints (typically 3-6)

Velocity naturally fluctuates. The useful planning range is not a single number but a range: the team's minimum, average, and maximum velocity over recent sprints.

### Capacity Planning

Capacity is the amount of work a team can take on in a specific upcoming sprint, accounting for individual availability. Capacity planning considers:

- **Individual Availability**: Days each team member is available (accounting for holidays, PTO, training, on-call rotations)
- **Load Factor**: Percentage of available time dedicated to sprint work (typically 60-80%, accounting for meetings, support, and overhead)
- **Allocation Percentage**: For members shared across teams, their fractional dedication to this team
- **Skill Constraints**: Whether available members have the skills needed for the planned work

### Skill Matrix

The skill matrix maps team members to technical and domain competencies, tracking proficiency levels and identifying gaps. It informs sprint planning (ensuring the team has the skills to deliver committed items), hiring decisions (identifying missing capabilities), and cross-training priorities (reducing single points of failure).

## Aggregate Roots

- **Team**: The primary aggregate root, containing team members, velocity history, capacity calculations, and the skill matrix

## Key Invariants

- Velocity is calculated only from completed sprints (not from in-progress or cancelled sprints)
- Team capacity cannot exceed the sum of individual member availability for a given sprint
- A team member cannot be allocated to more than their available capacity across all teams
- A team must have at least one member to be considered active
- Velocity and capacity are planning inputs, not performance targets

## Domain Events Produced

- TeamFormed -- A new team is created with initial membership
- MemberAdded -- A team member is added to the team
- MemberRemoved -- A team member departs the team
- VelocityCalculated -- Team velocity is recalculated after a sprint completes
- CapacityUpdated -- Team capacity is recomputed for an upcoming sprint
- SkillMatrixUpdated -- Team skill profile is modified

## Domain Events Consumed

- SprintCompleted (from Sprint Execution) -- Triggers velocity recalculation with new data point
- SprintStarted (from Sprint Execution) -- Locks capacity allocation for the sprint duration
- RetroCompleted (from Continuous Improvement) -- May update team health indicators

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Team & Capacity is one of the five agile bounded contexts
- [Sprint Execution Context](../sprint-execution-context/) -- Consumes velocity and capacity data for sprint planning
- [Backlog Management Context](../backlog-management-context/) -- Uses velocity for epic timeline forecasting
- [Aggregates](../aggregates/) -- Team is the aggregate root with TeamMember as an internal entity
- [Entities](../entities/) -- Team and TeamMember are entities with persistent identity and lifecycle
- [Value Objects](../value-objects/) -- Velocity, Capacity, Availability, and SkillMatrix are value objects
- [Domain Services](../domain-services/) -- VelocityCalculationService and CapacityPlanningService operate in this context

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." Sections: Scrum Team, Developers. 2020.
- Cohn, Mike. "Agile Estimating and Planning." Chapters 16-20: Velocity, Capacity, and Iteration Planning. Prentice Hall, 2005.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
