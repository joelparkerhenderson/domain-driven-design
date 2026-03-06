# Value Objects in Agile Management

## Overview

A value object is a domain object defined entirely by its attributes rather than by a unique identity. Value objects are immutable -- once created, they cannot be changed. If a different value is needed, a new value object is created. Two value objects with identical attributes are considered equal and interchangeable. In agile management, value objects model concepts like story points, velocity measurements, priorities, sprint goals, and workflow metrics that describe domain entities without having identity of their own.

## Relevance to Agile Management

Agile management involves many descriptive concepts that are naturally modeled as value objects. A story point estimate of "5" is the same regardless of which story it is attached to. A velocity of "32 points per sprint" is a measurement, not an identity. Priorities, acceptance criteria, definitions of done, cycle times, and WIP limits are all concepts defined by their values, not by a persistent identity. Modeling these as value objects rather than entities leads to simpler, safer, more testable code.

## Key Concepts

### Immutability

Value objects cannot be modified after creation. To change a story's estimate from 3 to 5 points, you create a new StoryPoint(5) value object and replace the old one -- you do not mutate StoryPoint(3) into StoryPoint(5). Immutability eliminates a large class of bugs related to shared mutable state.

### Equality by Value

Two StoryPoint objects with the value 3 are equal, regardless of which story they were originally created for. This contrasts with entities, where identity determines equality.

### Side-Effect-Free Behavior

Value objects can have methods, but those methods must be side-effect-free -- they return new value objects rather than modifying state. For example, `velocity.weightedAverage(newSprintPoints)` returns a new Velocity value object.

### Self-Validation

Value objects validate their own construction. A StoryPoint value object rejects negative values or values outside the Fibonacci sequence (if that is the team's convention). A WIPLimit rejects zero or negative values. This ensures that invalid values cannot exist in the domain.

## Agile Value Objects

### StoryPoint

- **Attributes**: Value (numeric, typically Fibonacci: 1, 2, 3, 5, 8, 13, 21)
- **Validation**: Must be a positive integer, optionally constrained to a defined estimation scale
- **Behavior**: Supports addition (summing story points for sprint capacity), comparison (for prioritization)
- **Usage**: Attached to UserStory entities in the Backlog Management Context; summed in the Sprint Execution Context for burndown tracking; averaged in the Team & Capacity Context for velocity calculation
- **Example**: `StoryPoint(5)` -- represents a medium-complexity story

### Velocity

- **Attributes**: PointsPerSprint (numeric), SprintCount (number of sprints in calculation), CalculationMethod (simple average, weighted average, rolling average)
- **Validation**: PointsPerSprint must be non-negative; SprintCount must be positive
- **Behavior**: Computes weighted averages, provides confidence intervals, compares velocities across time periods
- **Usage**: Calculated in the Team & Capacity Context; consumed by the Sprint Execution Context for planning
- **Example**: `Velocity(pointsPerSprint: 32, sprintCount: 6, method: WeightedAverage)` -- the team averages 32 points per sprint over the last 6 sprints

### Priority

- **Attributes**: Level (enum: Must, Should, Could, Wont for MoSCoW; or numeric RICE score), Rationale (optional text)
- **Validation**: Level must be a valid MoSCoW category or a non-negative RICE score
- **Behavior**: Supports comparison and sorting for backlog ordering
- **Usage**: Assigned to backlog items in the Backlog Management Context
- **MoSCoW Example**: `Priority(level: Must, rationale: "Regulatory requirement for Q2 release")`
- **RICE Example**: `Priority(reach: 5000, impact: 3, confidence: 80, effort: 2, score: 6000)`

### SprintGoal

- **Attributes**: Description (text), Measurable (boolean), AlignedObjective (optional reference)
- **Validation**: Description must be non-empty; should be concise (recommended under 100 characters)
- **Behavior**: Provides goal-completion assessment (met, partially met, not met)
- **Usage**: Set during Sprint Planning in the Sprint Execution Context
- **Example**: `SprintGoal(description: "Complete user authentication flow and integrate with SSO provider")`

### AcceptanceCriteria

- **Attributes**: Criteria (list of criterion strings), Format (Given-When-Then or checklist)
- **Validation**: Must contain at least one criterion; each criterion must be non-empty
- **Behavior**: Evaluates completeness (all criteria met vs. partial), supports Given-When-Then formatting
- **Usage**: Attached to UserStory entities in the Backlog Management Context; verified during sprint review
- **Example**: `AcceptanceCriteria(criteria: ["Given a registered user, When they enter valid credentials, Then they are redirected to the dashboard", "Given an unregistered email, When login is attempted, Then an error message is displayed"])`

### DefinitionOfDone

- **Attributes**: Checklist (list of criteria strings), Scope (Story-level, Sprint-level, Release-level)
- **Validation**: Must contain at least one item; items must be non-empty
- **Behavior**: Evaluates whether all checklist items are satisfied
- **Usage**: Applied at multiple scopes in the Sprint Execution Context; verified before story completion
- **Example**: `DefinitionOfDone(checklist: ["Code reviewed", "Unit tests passing", "Integration tests passing", "Documentation updated", "Deployed to staging"], scope: StoryLevel)`

### CycleTime

- **Attributes**: Duration (time span), StartEvent (workflow state entered), EndEvent (workflow state reached)
- **Validation**: Duration must be non-negative; EndEvent must be temporally after StartEvent
- **Behavior**: Supports statistical analysis (mean, median, percentile), trend detection
- **Usage**: Measured in the Continuous Improvement Context from Sprint Execution data
- **Example**: `CycleTime(duration: 3.5 days, startEvent: InProgress, endEvent: Done)`

### LeadTime

- **Attributes**: Duration (time span), RequestDate (when item entered backlog), DeliveryDate (when item was deployed)
- **Validation**: Duration must be non-negative; DeliveryDate must be after RequestDate
- **Behavior**: Supports statistical analysis, SLA compliance checking
- **Usage**: Measured in the Continuous Improvement Context across Backlog Management and Release Management data
- **Example**: `LeadTime(duration: 14 days, requestDate: 2024-01-15, deliveryDate: 2024-01-29)`

### WIPLimit

- **Attributes**: MaxItems (positive integer), WorkflowState (the state this limit applies to)
- **Validation**: MaxItems must be a positive integer
- **Behavior**: Validates whether adding a new item to a workflow state would exceed the limit; signals when limit is reached
- **Usage**: Applied to Kanban board columns in the Sprint Execution Context
- **Example**: `WIPLimit(maxItems: 3, workflowState: InProgress)` -- no more than 3 items can be "In Progress" simultaneously

### Estimate

- **Attributes**: Value (numeric), Unit (StoryPoints, Hours, Days, TShirtSize), Confidence (percentage), EstimatedBy (team or individual reference)
- **Validation**: Value must be non-negative; Confidence must be between 0 and 100
- **Behavior**: Supports conversion between estimation units (where applicable), confidence-weighted aggregation
- **Usage**: Created during Backlog Refinement, consumed during Sprint Planning
- **Example**: `Estimate(value: 8, unit: StoryPoints, confidence: 75, estimatedBy: "Team Alpha")`

### TimeBox

- **Attributes**: Duration (time span), StartDate, EndDate
- **Validation**: Duration must be positive; EndDate must equal StartDate plus Duration; Duration is immutable once Sprint is active
- **Behavior**: Calculates remaining time, validates that activities occur within the time-box
- **Usage**: Defines Sprint duration in the Sprint Execution Context
- **Example**: `TimeBox(duration: 2 weeks, startDate: 2024-02-05, endDate: 2024-02-16)`

### SemanticVersion

- **Attributes**: Major (non-negative integer), Minor (non-negative integer), Patch (non-negative integer), PreRelease (optional string)
- **Validation**: All version components must be non-negative integers; follows SemVer 2.0.0 specification
- **Behavior**: Supports comparison, increment (major/minor/patch), format to string
- **Usage**: Assigned to Release entities in the Release Management Context
- **Example**: `SemanticVersion(major: 2, minor: 4, patch: 1)` -- represents version 2.4.1

## Value Object Design Guidelines

1. **Make them immutable**: Never provide setters. All "modification" operations return new instances.
2. **Validate on construction**: Invalid value objects should never exist. Throw errors or return failure results from constructors.
3. **Define equality by attributes**: Override equality to compare attribute values, not references.
4. **Keep them simple**: Value objects should encapsulate a single concept. If a value object is growing complex, consider splitting it.
5. **Prefer value objects over primitives**: Use `StoryPoint(5)` instead of `int storyPoints = 5`. This prevents invalid values and adds domain meaning to the code.

## Relationships to Other Topics

- [Entities](../entities/) -- Entities contain value objects as descriptive attributes
- [Aggregates](../aggregates/) -- Value objects live within aggregate boundaries
- [Domain Events](../domain-events/) -- Domain events carry value objects as payload data
- [Ubiquitous Language](../ubiquitous-language/) -- Value object names come from the ubiquitous language
- [Domain Services](../domain-services/) -- Domain services operate on and return value objects

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software -- Value Objects. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6: Value Objects. Addison-Wesley, 2013.
- Cohn, Mike. "Agile Estimating and Planning." Chapters 4-6: Story Points, Velocity, and Estimation. Prentice Hall, 2005.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Chapters on WIP Limits and Flow Metrics. Blue Hole Press, 2010.
