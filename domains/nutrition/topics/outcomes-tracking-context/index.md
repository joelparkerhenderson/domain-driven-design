# Outcomes Tracking Context

## Overview

The Outcomes Tracking Context monitors the results of nutritional interventions over time, providing the evidence base for evaluating intervention effectiveness and informing clinical and coaching decisions. It tracks weight management trajectories and goal progress, analyzes biomarker trends for clinical decision support, assesses body composition changes (lean mass, fat mass, bone mineral density), and collects patient-reported outcomes such as perceived energy levels, appetite, and quality of life. This context aggregates data from all other bounded contexts into longitudinal views.

This bounded context is classified as a generic subdomain because it primarily performs data aggregation, trend analysis, and reporting, which are patterns common across healthcare and wellness domains. While the specific metrics tracked are nutrition-related, the underlying mechanisms for collecting, storing, and analyzing time-series health data do not require novel domain innovation.

## Ubiquitous Language

Within this context, "outcome" means a measurable health result attributable to nutritional intervention, not a process metric like compliance. "Trend" is a statistically characterized pattern in longitudinal data. "Timeline" is a chronological aggregation of data points for an individual. "Milestone" is a predefined outcome target that has been achieved. "Alert" is a notification triggered when a trend indicates clinical concern.

## Aggregates

**OutcomeTimeline** is the primary aggregate root. It assembles longitudinal health data for an individual across multiple metric types. It contains WeightRecord value objects, BodyCompositionMeasurement value objects, BiomarkerDataPoint value objects, and PatientReportedOutcome entities. The aggregate enforces chronological ordering of data points, validates data point ranges to catch recording errors (e.g., rejecting a weight of 5 kg for an adult), and maintains referential integrity between outcome data and the interventions that produced them.

## Entities

**PatientReportedOutcome** captures a subjective health measure reported by the individual at a specific time. It has a unique identifier, a timestamp, a measure type (energy level, appetite rating, gastrointestinal comfort, sleep quality, mood, overall quality of life), a scale value, and optional free-text notes. Each report is individually identified because tracking changes in subjective measures over time requires distinct instances with temporal context.

## Value Objects

**WeightRecord** captures body weight at a point in time with the measurement method (calibrated scale, self-reported), clothing state, and time of day. **BodyCompositionMeasurement** records fat mass percentage, lean mass in kilograms, bone mineral density, and the measurement method (DEXA, bioelectrical impedance, skinfold calipers, hydrostatic weighing). The measurement method is included because different methods produce systematically different values. **BiomarkerDataPoint** records a biomarker value at a timestamp, carrying the biomarker type, value, unit, reference range, and source context. **TrendDataPoint** is a generic time-series point with timestamp, numeric value, unit, and data source.

## Domain Events

**OutcomeMilestoneReached** is raised when a tracked outcome achieves a predefined goal, such as reaching a target weight, normalizing a deficient biomarker, or achieving a body composition target. This event is consumed by all other contexts to inform plan adjustments and celebrate progress with the individual.

**TrendAlertTriggered** is raised when longitudinal trend analysis detects a concerning pattern: weight loss exceeding safe rates (more than 1% of body weight per week without clinical indication), progressive biomarker deterioration, or declining patient-reported outcomes over consecutive periods. This event notifies Clinical Nutrition and the individual's care team.

**OutcomeDataPointRecorded** is raised when any new data point is added to a timeline, enabling real-time dashboarding and notification systems.

## Domain Services

**TrendAnalysisService** performs longitudinal analysis on outcome data. It calculates moving averages to smooth data variability, computes rates of change to identify acceleration or deceleration of trends, applies statistical tests to determine whether observed changes are significant versus normal fluctuation, and generates trend alerts when patterns indicate clinical concern. The service operates across multiple data point types within the OutcomeTimeline aggregate.

**OutcomeCorrelationService** analyzes relationships between different outcome metrics and intervention data. For example, it may identify that compliance scores above 80% correlate with weight loss of 0.5 kg/week, or that a specific dietary pattern change preceded biomarker improvement. This service provides evidence for intervention effectiveness.

## Repositories

**OutcomeTimelineRepository** provides access to timeline aggregates with operations for finding by subject, querying data points by metric type and date range, and retrieving timelines with active alerts.

## Integration Points

The Outcomes Tracking context is downstream of all other contexts, receiving events that contribute data points to longitudinal tracking. It receives AssessmentCompleted and BiomarkerResultRecorded from Nutritional Assessment, MealPlanActivated and MealPlanModified from Meal Planning, NutritionDiagnosisEstablished and DietPrescriptionIssued from Clinical Nutrition, FuelingPhaseTransitioned and REDSRiskDetected from Sports Nutrition, and ComplianceScoreCalculated from Dietary Compliance. This positions Outcomes Tracking as the aggregating context in the domain architecture.

## Data Quality Considerations

Outcome data quality depends on measurement consistency, instrument calibration, and data collection frequency. The domain model captures measurement methodology with each data point to enable appropriate comparisons. DEXA body composition measurements should not be directly compared with bioelectrical impedance measurements in trend analysis. Self-reported weights carry different confidence levels than clinical scale measurements.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Lachat, Carl, et al. "Strengthening the Reporting of Observational Studies in Epidemiology-Nutritional Epidemiology (STROBE-nut)." PLOS Medicine, 2016.
