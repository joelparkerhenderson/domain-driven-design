# Progress Tracking Context

## Overview

The Progress Tracking Context manages the longitudinal dimension of body health data, transforming measurement histories into personalized health improvement narratives. It owns goal setting, trend analysis, milestone tracking, and streak management -- the behavioral engagement layer that motivates sustained health improvement. This context represents a core subdomain, as the intelligence embedded in goal recommendation, adaptive progress analysis, and achievement recognition directly drives user engagement and health outcomes.

## Scope and Responsibilities

This context manages the following capabilities:

- **Goal Setting**: Creation and management of specific, measurable health improvement objectives with defined timelines and target values
- **Trend Analysis**: Statistical analysis of measurement changes over time, distinguishing meaningful change from measurement noise
- **Milestone Tracking**: Recognition of predefined achievement points within longer-term health improvement plans
- **Streak Management**: Tracking consecutive adherence to measurement schedules or activity targets
- **Progress Visualization Data**: Computation of data points for progress charts, trend lines, and comparative displays
- **Goal Recommendation**: Evidence-based suggestion of health improvement targets based on current measurements and population guidelines

## Aggregate Design

### HealthGoal (Aggregate Root)

The HealthGoal aggregate manages an individual's health improvement objective from creation through completion or abandonment.

**Components**:
- TargetMetric (value object): the specific measurement type being targeted (e.g., body weight, body fat percentage, VO2 max)
- GoalTimeline (value object): start date, target date, and intermediate checkpoint dates
- CurrentProgress (value object): current metric value, percentage toward target, projected completion date
- Milestones: ordered collection of milestone definitions with achievement status
- ProgressHistory: chronological record of measurement-to-goal evaluations

**Invariants**:
- Target value must be physiologically achievable (e.g., cannot target body fat below 2%)
- Goal timeline must have a start date on or before the target date
- Milestones must be ordered by value and non-overlapping
- Progress percentage must be between 0% and 100%
- A completed or abandoned goal cannot accept further progress updates

### MeasurementStreak (Aggregate Root)

The MeasurementStreak aggregate tracks consecutive adherence to a measurement or activity schedule.

**Components**:
- StreakType (value object): the category of streak being tracked (daily weigh-in, weekly assessment, activity target)
- StreakLength (value object): current number of consecutive qualifying periods
- StreakHistory: record of all periods with qualification status
- GracePeriod (value object): allowed gap before a streak is considered broken

**Invariants**:
- Streak length must be non-negative
- A broken streak resets to zero (or to a configured minimum)
- Grace period rules must be applied consistently across all streak evaluations
- Streak qualification criteria must be defined at creation and remain immutable

### ProgressTrend (Value Object)

Represents the statistical analysis of a metric's trajectory over time.

**Attributes**:
- Direction: improving, declining, stable
- Rate of change: units per time period
- Statistical confidence: confidence interval for the detected trend
- Projected value at goal date
- Noise-filtered measurement series

## Domain Events Published

- **MilestoneAchieved**: Raised when an individual reaches a predefined achievement point, carrying the milestone details and metric value at achievement
- **GoalCompleted**: Raised when the target metric value is achieved within the goal timeline
- **GoalAbandoned**: Raised when a goal is explicitly abandoned or expires without completion
- **StreakBroken**: Raised when a consecutive adherence streak is interrupted
- **StreakMilestoneReached**: Raised when a streak reaches a notable length (7 days, 30 days, 100 days)
- **TrendDirectionChanged**: Raised when trend analysis detects a reversal in measurement trajectory (e.g., from improving to declining)
- **GoalProgressStalled**: Raised when no meaningful progress has been detected within a configured period, potentially triggering goal reassessment

## Domain Services

- **TrendAnalysisService**: Applies statistical methods (linear regression, moving averages, change-point detection) to measurement histories to detect meaningful trends and filter measurement noise
- **GoalRecommendationService**: Recommends health improvement goals based on current measurements, population guidelines, and individual measurement history
- **MilestoneCalibrationService**: Sets milestone values that are challenging but achievable based on evidence from similar individuals' progress patterns
- **ProjectionService**: Estimates future metric values and goal completion dates based on current trend data

## Integration Points

- **Upstream**: Consumes BiometricMeasurementRecorded events from Biometric Collection, CompositionProfileUpdated from Body Composition, FitnessAssessmentCompleted from Fitness Assessment, and HealthScoreCalculated from Health Scoring
- **Downstream**: Publishes milestone and streak events consumed by notification and engagement systems

## Ubiquitous Language (Context-Specific)

- **Health Goal**: A specific, measurable, time-bound target for a body health metric
- **Milestone**: An intermediate achievement point within a longer-term health improvement plan
- **Streak**: Consecutive qualifying periods of adherence to a measurement or activity target
- **Grace Period**: An allowed gap (e.g., one day) before a streak is considered broken, accommodating real-world variability
- **Trend Direction**: The overall trajectory of a metric over time: improving (moving toward goal), declining (moving away from goal), or stable
- **Measurement Noise**: Short-term fluctuations in measurements that do not represent meaningful physiological change (e.g., daily weight variation due to hydration)
- **Stalled Progress**: A period during which no statistically significant change toward the goal is detected

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Locke, E.A. and Latham, G.P. "Building a practically useful theory of goal setting and task motivation." *American Psychologist*, 57(9), 2002.
