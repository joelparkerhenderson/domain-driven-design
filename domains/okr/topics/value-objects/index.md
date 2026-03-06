# Value Objects

## Overview

In Domain-Driven Design, a Value Object is an immutable domain object defined entirely by its attributes rather than by a unique identity. Two value objects with identical attributes are considered equal and interchangeable. Value objects encapsulate domain concepts, enforce validation rules at construction time, and make implicit concepts explicit.

In the OKR domain, value objects represent the measurements, classifications, temporal boundaries, and qualitative assessments that give meaning to objectives and key results. They ensure that invalid states are unrepresentable and that domain rules are enforced consistently wherever these concepts appear.

## OKR Domain Value Objects

### Score

**Type**: Decimal (0.0 to 1.0)

**Description**: A Score represents the degree to which a Key Result has been achieved. It is the fundamental unit of OKR measurement. Following Google's OKR methodology, a score of 0.7 is typically considered successful for aspirational objectives, while committed objectives are expected to reach 1.0.

**Validation Rules**:
- Must be a decimal value between 0.0 and 1.0 inclusive.
- Precision is limited to two decimal places.
- A score of 0.0 indicates no progress; 1.0 indicates full achievement.

**Usage**: Key Result Tracking Context (scoring), Review & Cadence Context (grading), Analytics & Reporting Context (aggregation).

### ConfidenceLevel

**Type**: Enumeration (High, Medium, Low)

**Description**: A ConfidenceLevel captures the owner's subjective assessment of the likelihood of achieving a Key Result by the end of the cycle. It serves as an early warning signal during check-ins.

**Semantics**:
- **High**: On track to achieve the target with current trajectory.
- **Medium**: Achievement is possible but requires attention or intervention.
- **Low**: Unlikely to achieve the target without significant course correction.

**Usage**: Key Result Tracking Context (check-ins), Review & Cadence Context (review sessions).

### ProgressUpdate

**Type**: Composite (CurrentValue: Decimal, UpdatedAt: DateTime, UpdatedBy: PersonId, Narrative: String)

**Description**: A ProgressUpdate records a point-in-time measurement of a Key Result's current value along with contextual narrative. Each update is immutable once recorded. The collection of ProgressUpdates for a Key Result forms its progress history.

**Validation Rules**:
- CurrentValue must be a valid number within the Key Result's measurement range.
- Narrative is optional but recommended; if provided, it must be under 1000 characters.
- UpdatedAt must fall within the associated cycle's active DateRange.

### DateRange

**Type**: Composite (StartDate: Date, EndDate: Date)

**Description**: A DateRange defines the temporal boundaries for cycles, objectives, and other time-bounded OKR activities. It ensures that start and end dates are logically consistent.

**Validation Rules**:
- StartDate must precede EndDate.
- The range must span at least one day.
- DateRange comparisons support containment checks (e.g., an Objective's DateRange must fall within its Cycle's DateRange).

### Priority

**Type**: Enumeration (P0, P1, P2)

**Description**: Priority indicates the relative importance of an Objective within its owner's set of objectives for a given cycle. It helps focus attention and resource allocation.

**Semantics**:
- **P0**: Must-achieve; critical to organizational strategy.
- **P1**: Should-achieve; important but secondary to P0 objectives.
- **P2**: Nice-to-achieve; stretch goals or exploratory objectives.

### SMARTCriteria

**Type**: Composite (Specific: Boolean, Measurable: Boolean, Actionable: Boolean, Relatable: Boolean, Timely: Boolean, ValidationNotes: String)

**Description**: SMARTCriteria records whether a Key Result satisfies each dimension of the SMART framework: Specific, Measurable, Actionable, Relatable, and Timely. This value object is applied during objective setting to ensure quality Key Results.

**Validation Rules**:
- All five Boolean fields must be true for the Key Result to pass SMART validation.
- ValidationNotes provides human-readable justification for each criterion's assessment.

**Usage**: Objective Setting Context (definition and approval gates).

### OKRType

**Type**: Enumeration (Committed, Aspirational)

**Description**: OKRType classifies an Objective by its expected achievement level. This distinction, emphasized by Doerr, drives different scoring expectations and accountability models.

**Semantics**:
- **Committed**: Expected to be fully achieved (score of 1.0). Failure to achieve indicates planning or execution problems.
- **Aspirational**: Intentionally ambitious. A score of 0.6-0.7 is considered successful. These objectives push teams beyond comfortable limits.

**Immutability Note**: OKRType is set at Objective creation and cannot change after the Objective becomes Active.

### Cadence

**Type**: Composite (Frequency: Enumeration [Weekly, Biweekly, Monthly], DayOfWeek: Enumeration [Monday-Friday])

**Description**: Cadence defines the recurring rhythm for check-ins within an OKR cycle. It specifies how often progress reviews occur and on which day they are scheduled.

**Validation Rules**:
- Frequency must produce at least two check-in opportunities within the associated cycle's DateRange.
- DayOfWeek must be a business day (Monday through Friday).

## Value Object Design Principles

### Immutability

All OKR value objects are immutable. A Score of 0.7 never changes; if a new measurement yields 0.8, a new Score value object is created. This immutability simplifies reasoning, enables safe sharing across aggregates, and supports event-sourced architectures.

### Self-Validation

Value objects validate their invariants at construction time. It is impossible to create a Score of 1.5 or a DateRange where the start follows the end. This eliminates an entire class of domain errors.

### Equality by Value

Two ConfidenceLevel objects representing "High" are identical and interchangeable. This contrasts with entities, where two Objectives with the same title are still distinct because they have different ObjectiveIds.

## Relationships to Other Topics

- **Entities**: Value objects are embedded within entities; for example, an Objective contains Priority and OKRType. See [Entities](../entities/).
- **Aggregates**: Value objects participate in aggregate invariant enforcement. See [Aggregates](../aggregates/).
- **Domain Events**: Events carry value objects as payload data (e.g., ScoreFinalized carries a Score). See [Domain Events](../domain-events/).
- **Objective Setting Context**: SMARTCriteria and OKRType are central to [Objective Setting](../objective-setting-context/).
- **Key Result Tracking Context**: Score, ConfidenceLevel, and ProgressUpdate drive [Key Result Tracking](../key-result-tracking-context/).
- **OKR Standards**: SMARTCriteria directly implements the SMART framework. See [OKR Standards](../okr-standards/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software (Value Objects).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 6: Value Objects.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018. Chapters on scoring and committed vs. aspirational OKRs.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
