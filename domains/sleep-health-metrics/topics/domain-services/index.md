# Domain Services - Sleep Health Metrics

## Overview

Domain services encapsulate stateless operations that naturally span multiple aggregates or do not belong to any single entity or value object. In the sleep health metrics domain, domain services perform computations that require data from multiple aggregates, coordinate cross-aggregate workflows, and implement business logic that does not fit within a single aggregate's responsibility.

## Domain Service Design Principles

Domain services in this domain are stateless: they receive input, perform computation, and return results without maintaining internal state between invocations. They are expressed in the ubiquitous language of sleep medicine. They operate on domain objects (entities, value objects, aggregates) and produce domain objects. They do not access infrastructure directly -- they receive data through parameters and return results rather than reading from databases or calling external APIs.

## Sleep Stage Analysis Services

### SleepArchitectureCalculationService

This service computes sleep architecture metrics from a complete set of scored epochs. It spans the ScoringSession aggregate and produces SleepArchitectureSummary value objects.

Operations include:

- calculateStageDistribution(scoredEpochs): Computes percentage and duration of each sleep stage (W, N1, N2, N3, R) from the set of scored epochs.
- identifySleepCycles(scoredEpochs): Detects NREM-REM sleep cycles using standard cycle detection algorithms, computing cycle count, average cycle duration, and cycle-to-cycle variability.
- computeStageTransitionMatrix(scoredEpochs): Generates a transition probability matrix showing the likelihood of moving from one stage to another, useful for identifying abnormal stage transition patterns.

### RespiratoryEventClassificationService

This service applies AASM rules to classify respiratory events and compute severity indices.

Operations include:

- classifyEvent(respiratorySignal, oxygenDesaturation, effortSignals): Determines whether a respiratory event is an obstructive apnea, central apnea, mixed apnea, hypopnea, or RERA based on AASM criteria.
- computeAHI(events, totalSleepTime): Calculates the apnea-hypopnea index from classified events and total sleep time, applying positional and REM-specific breakdowns.
- assessSeverity(ahi): Classifies obstructive sleep apnea severity (normal, mild, moderate, severe) based on the computed AHI.

## Sleep Quality Assessment Services

### CompositeScoreCalculationService

This service computes standardized sleep quality indices from component data. It spans multiple aggregates (QualityAssessment, SubjectiveAssessment, ScoringSession).

Operations include:

- calculatePSQI(responses): Computes the seven PSQI component scores and the global score from patient questionnaire responses, applying the scoring algorithm defined by Buysse et al. (1989).
- calculateESS(responses): Computes the Epworth Sleepiness Scale total score from situation-specific sleepiness ratings, applying the scoring algorithm defined by Johns (1991).
- interpretScore(instrument, score): Provides clinical interpretation of a composite score based on published thresholds (e.g., PSQI global score greater than 5 indicates poor sleep quality).

### LongitudinalTrendAnalysisService

This service analyzes sleep quality metrics over time to detect clinically significant trends.

Operations include:

- analyzeTrend(metricSeries, windowSize): Computes trend direction (improving, stable, deteriorating) for a time series of a specific metric (sleep efficiency, WASO, AHI) using statistical methods appropriate for clinical time series.
- detectSignificantChange(baseline, current, clinicalThreshold): Determines whether the change between baseline and current metric values exceeds a clinically meaningful threshold.
- generateTrendSummary(patientId, dateRange): Produces a comprehensive trend summary across all quality metrics for a patient, suitable for clinical review.

## Circadian Rhythm Services

### CircadianPhaseEstimationService

This service estimates circadian phase from available markers when direct DLMO measurement is not available.

Operations include:

- estimatePhaseFromActigraphy(activityData, lightData): Estimates circadian phase from rest-activity patterns and light exposure data using established algorithms (e.g., cosinor analysis).
- calculateSocialJetLag(workDaySleepTiming, freeDaySleepTiming): Computes the discrepancy between work-day and free-day sleep midpoints.
- assessCircadianAlignment(circadianPhase, actualSleepTiming): Quantifies the degree of alignment between estimated circadian phase and actual sleep timing, identifying phase advance or delay.

## Intervention Efficacy Services

### InterventionEfficacyService

This service evaluates the effectiveness of interventions by comparing pre-intervention baseline metrics with post-intervention outcomes. It spans InterventionPlan and QualityAssessment aggregates.

Operations include:

- evaluateCBTIOutcome(baselineMetrics, postTreatmentMetrics): Compares sleep efficiency, sleep latency, WASO, and PSQI scores before and after CBT-I treatment, applying established response criteria (e.g., remission defined as PSQI less than or equal to 5 and ISI less than 8).
- evaluateCPAPEfficacy(preAHI, adherenceData, residualAHI): Assesses CPAP treatment success by comparing pre-treatment AHI with residual AHI under therapy, factoring in adherence levels.
- recommendAdjustment(interventionPlan, currentMetrics, targetMetrics): Suggests intervention adjustments when current metrics have not reached target thresholds, based on clinical decision rules.

### MultiModalTreatmentCoordinationService

This service coordinates treatment plans that combine multiple intervention modalities.

Operations include:

- assessInteractionRisk(medications, deviceTherapy): Evaluates potential interactions between pharmacological and device-based therapies (e.g., sedating medications affecting CPAP tolerance).
- optimizeTreatmentSequencing(interventionModules, circadianProfile): Suggests optimal timing and sequencing of interventions based on circadian phase alignment.

## Clinical Integration Services

### ReportGenerationService

This service assembles clinical reports from data spanning multiple bounded contexts.

Operations include:

- generateSleepStudyReport(studyId, scoringSessionId, assessmentId): Compiles a comprehensive sleep study report including recording details, scoring results, quality metrics, clinical impressions, and recommendations.
- mapToFHIR(domainObject): Translates a domain aggregate or value object into the corresponding FHIR R4 resource representation.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 5: Domain Services.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 7: Services.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 10: Domain Services.
- Buysse, D. J. et al. (1989). The Pittsburgh Sleep Quality Index. *Psychiatry Research*, 28(2), 193-213.
- Johns, M. W. (1991). The Epworth Sleepiness Scale. *Sleep*, 14(6), 540-545.
