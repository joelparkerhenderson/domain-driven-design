# Injury Prevention Context - Kinesiology Domain

## Overview

The Injury Prevention Context synthesizes data from multiple sources to proactively identify individuals at elevated risk of injury and implement strategies to mitigate that risk. Unlike the Rehabilitation Planning Context, which operates reactively after injury occurs, the Injury Prevention Context operates predictively, using screening data, workload metrics, movement quality indicators, and injury history to flag risk before injury manifests.

Injury prevention is classified as a supporting subdomain. It extends the core assessment and prescription capabilities with specialized risk management functionality. The domain logic draws heavily on sports injury epidemiology research and evidence-based prevention strategies.

## Key Concepts

Screening protocols are systematic assessment procedures designed to identify individuals at elevated injury risk based on movement quality, strength ratios, flexibility, and sport-specific demands. Screens may include the Functional Movement Screen (FMS), the Y-Balance Test, landing mechanics assessment, and sport-specific risk factor evaluations.

Movement correction addresses identified movement deficits through targeted exercise interventions. When screening identifies compensatory patterns, asymmetries, or mobility restrictions, the Injury Prevention Context generates correction strategies that may be communicated to the Exercise Prescription Context for integration into training programs.

Load management is the strategic regulation of training volume, intensity, and frequency to maintain the balance between performance adaptation and injury risk. Load management uses quantitative workload monitoring to ensure that training progressions do not exceed the body's capacity to adapt, which is a primary mechanism of overuse injury.

Acute-to-chronic workload ratio (ACWR) is the primary workload monitoring metric. It compares recent training load (typically the past 7 days) to longer-term average training load (typically the past 28 days). Research has identified an ACWR range of 0.8 to 1.3 as the zone of lowest injury risk, with ratios above 1.5 associated with significantly elevated risk.

Prehabilitation is proactive exercise programming that targets known vulnerability areas to reduce future injury risk before symptoms or injury occur. Prehabilitation programs are designed based on screening results, sport-specific injury epidemiology, and individual risk factor profiles.

## Aggregate Design

The InjuryRiskProfile aggregate is the primary aggregate root. It aggregates all relevant risk factors for an individual and tracks risk level changes over time. The aggregate enforces that risk calculations are based on current, valid screening data, that alert thresholds trigger appropriate notifications when risk levels exceed defined limits, and that workload metrics reflect the actual training history.

Key invariants include: risk scores must be recalculated whenever new screening data or workload data is integrated, alert thresholds must be evaluated after every risk score update, screening data older than a defined validity period must be flagged as expired, and ACWR calculations must use the correct rolling window parameters.

## Entities

The InjuryRiskProfile entity carries a persistent identity, tracking an individual's evolving risk profile over time and maintaining the complete history of risk factor assessments.

The ScreeningRecord entity represents a specific administration of a screening protocol, individually identifiable for longitudinal tracking and for correlating screening results with subsequent injury outcomes.

The WorkloadEntry entity represents a single training load observation, recording the date, session type, duration, and load metric. Accumulated workload entries form the basis for ACWR calculations.

The PrehabilitionProgram entity represents a preventive exercise program designed to address identified risk factors. It has a lifecycle from prescribed through active to completed or superseded.

## Value Objects

AcuteChronicWorkloadRatio captures the calculated ratio with its acute window, chronic window, calculation method (rolling average or exponentially weighted moving average), and the resulting ratio value.

RiskScore represents a composite injury risk level with the calculated score, the risk classification (low, moderate, high, very high), and the contributing factors that determined the score.

AlertThreshold defines the risk score level that triggers a notification, including the threshold value, the alert severity, and the recommended response.

ScreeningScore captures the result of a single screening test with the test name, score value, normative percentile, and clinical significance classification.

WorkloadMetric represents a calculated training load value for a specific time period, including the metric type (session RPE, tonnage, distance), the time window, and the calculated value.

## Domain Events

RiskAlertRaised is published when an individual's risk profile exceeds a defined threshold, carrying the risk score, contributing factors, and recommended actions. The Exercise Prescription Context consumes this to trigger program modifications.

WorkloadAlertRaised is published when ACWR moves outside the recommended range, carrying the current ratio, trend direction, and recommended load adjustment. This is a critical safety event.

ScreeningCompleted is published when a screening protocol is completed, carrying screening results, identified risk factors, and recommended follow-up.

RiskProfileUpdated is published when an individual's risk profile is recalculated based on new data, enabling longitudinal risk tracking across the system.

## Domain Services

RiskCalculationService calculates composite injury risk scores by aggregating risk factors from screening results, workload data, movement quality indicators, and injury history, applying evidence-based weighting.

WorkloadMonitoringService continuously monitors training load patterns and calculates ACWR, monotony indices, and strain calculations, identifying dangerous workload patterns.

PrehabilitionRecommendationService generates prehabilitation exercise recommendations by matching risk profiles to evidence-based preventive interventions.

## Integration Points

Injury Prevention is downstream of Movement Assessment, consuming screening and assessment data through a published language contract. It is downstream of Exercise Prescription, consuming training load data to monitor workload patterns. It is downstream of Performance Analysis, consuming performance metrics to identify fatigue and detraining indicators. It publishes risk alerts upstream to Exercise Prescription to trigger program modifications.

## Epidemiological Evidence Base

Injury prevention strategies are grounded in sports injury epidemiology research. The van Mechelen model (1992) provides the framework for injury prevention research: establish injury incidence, identify risk factors, implement preventive measures, and evaluate effectiveness. The ACWR model draws on the work of Gabbett (2016) and subsequent refinements.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Gabbett, Tim J. "The Training-Injury Prevention Paradox." British Journal of Sports Medicine, 2016.
- van Mechelen, Willem, et al. "Training and Sports Injuries." British Journal of Sports Medicine, 1992.
- Bahr, Roald, and Ingar Holme. "Risk Factors for Sports Injuries." British Journal of Sports Medicine, 2003.
