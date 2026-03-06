# Outcomes Tracking Context

## Overview

The Outcomes Tracking Context manages longitudinal sleep quality outcomes and supports clinical decision-making through trend analysis and reporting. This context aggregates data from assessments, device measurements, intervention progress, and patient-reported outcomes to build a comprehensive temporal view of each individual's sleep quality journey. It identifies trends, detects milestones, and provides the analytical foundation for evaluating whether interventions are producing desired results.

## Core Responsibilities

This context owns the domain logic for longitudinal outcome measurement, trend computation, and outcomes reporting. It consumes events from all other bounded contexts to build a unified temporal record. It computes composite quality scores from multiple data sources. It performs trend analysis to identify improvement, stability, or deterioration patterns. It detects milestone achievement (such as sustained sleep efficiency above 85 percent). It publishes trend and milestone events for clinical alerting and treatment review.

## Outcome Metrics

### Sleep Efficiency Trends

Sleep efficiency (total sleep time divided by time in bed) is tracked over time as both nightly values and rolling averages (7-day, 14-day, 30-day). The context computes trend direction, rate of change, and variability. Clinically meaningful improvement is defined as a sustained increase of 5 or more percentage points from baseline over at least two weeks.

### Composite Sleep Quality Score

The Composite Quality Score integrates multiple metrics into a single index using a weighted formula. Components include sleep efficiency (weight: primary), sleep onset latency (weight: secondary), WASO (weight: secondary), total sleep time relative to age-appropriate target (weight: secondary), subjective quality rating (weight: secondary), and daytime functioning indicators (weight: tertiary). The weighting can be configured for different clinical contexts.

### Daytime Functioning Assessment

Daytime functioning tracks the impact of sleep on waking life across multiple dimensions: cognitive performance (concentration, memory, decision-making), mood (irritability, anxiety, depression symptoms), energy level (fatigue, motivation), and social functioning (relationship impact, work performance). These are captured through periodic patient-reported outcome measures.

### Patient Satisfaction

Patient satisfaction measures the individual's subjective evaluation of their sleep quality improvement journey, including satisfaction with treatment approach, perceived progress, and overall experience. This is tracked through periodic satisfaction questionnaires and contributes to the composite quality score.

## Key Aggregates

### Outcomes Record

The primary aggregate maintains a temporal series of outcome measurements for an individual. The root entity links to the patient identifier and tracks the record start date, most recent measurement date, and aggregate trend statistics. It contains an ordered collection of OutcomeMeasurement value objects, each capturing a point-in-time snapshot of all tracked metrics. The aggregate enforces chronological ordering, prevents duplicate measurements for the same date, and recomputes trend statistics when new measurements are added.

## Trend Analysis

The context performs several types of trend analysis on outcome time series. Direction analysis determines whether metrics are improving, stable, or deteriorating over a specified window. Rate analysis computes the velocity of change (percentage points per week for sleep efficiency, score points per month for composite scores). Variability analysis measures night-to-night or week-to-week consistency, as high variability may indicate instability even when average values are acceptable. Correlation analysis examines the temporal relationship between intervention activities and outcome changes.

## Milestone Detection

The context defines and monitors predefined milestones that represent clinically meaningful achievements. Examples include: sleep efficiency consistently above 85 percent for two consecutive weeks, sleep onset latency consistently under 20 minutes for two weeks, PSQI global score improved by 3 or more points from baseline, and CPAP usage averaging 4 or more hours per night for 30 days (Medicare compliance threshold). When a milestone is reached, the context publishes a QualityMilestoneReached event.

## Baseline and Follow-Up

The context distinguishes between baseline measurements (captured before intervention begins) and follow-up measurements (captured during and after intervention). This distinction is essential for computing meaningful change scores and for evaluating intervention effectiveness. The context tracks which measurements are baseline versus follow-up and computes change from baseline for each metric.

## Domain Events Published

The context publishes OutcomeTrendIdentified when a significant trend is detected, carrying the metric type, trend direction, duration, and statistical confidence. It publishes QualityMilestoneReached when a predefined milestone is achieved. These events support clinical alerting (notifying clinicians of deterioration), patient engagement (celebrating improvements), and treatment review (triggering protocol adjustment discussions).

## Domain Events Consumed

The context consumes events from all other bounded contexts: AssessmentCompleted and SleepDiaryEntryRecorded from Sleep Assessment, DisorderDiagnosed and DisorderSeverityChanged from Sleep Disorders, InterventionProtocolStarted and SessionCompleted from Behavioral Interventions, DeviceDataReceived from Device Integration, and EnvironmentAssessmentCompleted from Sleep Environment. Each consumed event contributes data to the outcome record.

## Reporting

The context supports multiple reporting views: individual patient outcome reports (for clinical review), cohort outcome reports (for population health analysis), intervention effectiveness reports (comparing outcomes across intervention types), and research data exports (for clinical research use). Reports are generated from the outcome record aggregates without modifying them.

## Data Completeness

The context tracks data completeness for each outcome record, monitoring whether expected data sources (diary entries, device data, assessment scores) are being received at the expected frequency. Gaps in data are flagged but do not prevent trend analysis; the context computes trends from available data and annotates results with completeness indicators.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Morin, C.M. et al. (2011). The Insomnia Severity Index: Psychometric Indicators to Detect Insomnia Cases and Evaluate Treatment Response. Sleep, 34(5), 601-608.
