# Domain Services in the Sleep Quality Domain

## Overview

Domain services are stateless operations that encapsulate domain logic which does not naturally belong to any single entity or value object. When a significant domain operation spans multiple aggregates, involves coordination between bounded contexts, or represents a pure domain computation that does not fit within an entity's responsibility, a domain service is the appropriate modeling choice. In the sleep quality domain, domain services handle cross-aggregate computations, assessment scoring, treatment selection, and outcome analysis.

## Assessment Scoring Service

The AssessmentScoringService encapsulates the scoring algorithms for validated sleep quality instruments. While individual instrument results are value objects, the process of computing scores from raw questionnaire responses involves domain logic that does not belong to any single entity. The service accepts raw response data and produces scored result value objects.

For the PSQI, the service computes seven component scores from 19 questionnaire items using the published scoring algorithm, including the complex rules for component 5 (sleep disturbances) which involves summing multiple sub-items. For the ESS, the service sums eight situation scores. For the ISI (Insomnia Severity Index), the service computes the total from seven items and classifies severity. The scoring logic is centralized in this service to ensure consistency and to facilitate updates when instrument scoring rules are revised.

## Sleep Restriction Calculation Service

The SleepRestrictionCalculationService computes the prescribed time-in-bed window for sleep restriction therapy based on sleep diary data. This operation spans the Sleep Diary aggregate (in the Assessment Context) and the Intervention Protocol aggregate (in the Behavioral Interventions Context). The service calculates average total sleep time from recent diary entries, applies the sleep restriction algorithm (initial time-in-bed equals average total sleep time, with a minimum of 5 hours), and determines when to adjust the window based on sleep efficiency trends.

The service enforces the clinical rules: if sleep efficiency exceeds 90 percent for a week, time-in-bed is increased by 15-30 minutes; if sleep efficiency falls below 80 percent, time-in-bed is reduced by 15-30 minutes; if sleep efficiency is between 80 and 90 percent, the window remains unchanged. This logic crosses aggregate boundaries and involves domain expertise that is best captured in a dedicated service.

## Disorder Eligibility Service

The DisorderEligibilityService evaluates whether assessment data meets diagnostic criteria for specific sleep disorders. This service coordinates between assessment results (from the Assessment Context) and diagnostic criteria (from the Disorders Context). Given a set of assessment results and clinical observations, the service evaluates each diagnostic criterion and produces an eligibility determination.

For insomnia disorder, the service checks frequency (at least three nights per week), duration (at least three months), and impact on daytime functioning. For obstructive sleep apnea, it evaluates AHI thresholds in conjunction with symptom criteria. The service does not make the diagnosis (which remains a clinical decision modeled in the Disorder Diagnosis entity) but provides the structured evaluation that supports the decision.

## Composite Quality Score Service

The CompositeQualityScoreService computes an aggregate sleep quality score from multiple data sources. This service operates within the Outcomes Tracking Context but draws on data originating from multiple contexts (assessment scores, device metrics, diary entries, intervention adherence). The service applies a weighted formula that combines sleep efficiency, sleep onset latency, WASO, total sleep time, subjective quality ratings, and daytime functioning indicators into a single composite score.

The weighting algorithm is a core domain decision that reflects clinical priorities. The service encapsulates this algorithm and supports configuration of weights for different clinical contexts (research studies may weight objective measures more heavily; clinical practice may give more weight to subjective reports and daytime functioning).

## Environment Recommendation Service

The EnvironmentRecommendationService generates evidence-based environmental optimization recommendations from environmental readings. Given a set of environmental measurements (light, noise, temperature, humidity, air quality), the service evaluates each against evidence-based thresholds and produces prioritized recommendations. This service spans the Environmental Reading value objects and the Recommendation value objects within the Environment Assessment aggregate.

## Device Data Normalization Service

The DeviceDataNormalizationService handles the transformation of raw device data into domain-standard formats. This stateless service applies device-specific transformation rules (maintained by the anti-corruption layer configuration) to convert proprietary data representations into canonical domain value objects. The service handles unit conversion, timestamp normalization, data quality assessment, and missing data interpolation.

## Trend Analysis Service

The TrendAnalysisService performs statistical analysis on longitudinal outcome data to identify significant trends. Given a time series of outcome measurements, the service computes moving averages, trend direction and slope, statistical significance of changes, and milestone achievement detection. This service supports the Outcomes Tracking Context's ability to publish OutcomeTrendIdentified and QualityMilestoneReached domain events.

## Service Design Principles

Domain services in the sleep quality domain are stateless, meaning they do not retain any data between invocations. All necessary input is provided as parameters, and all output is returned as result objects. Services are defined as interfaces in the domain layer and implemented in the application or infrastructure layer as appropriate. Services that involve pure domain computation (scoring algorithms, eligibility evaluation) are implemented in the domain layer. Services that coordinate across bounded contexts are implemented in the application layer.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on domain services.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 7 on domain services.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 10 on domain services.
- Morin, C.M. et al. (2006). Cognitive Behavioral Therapy for Insomnia. Sleep Medicine Reviews, 10(5), 281-295.
