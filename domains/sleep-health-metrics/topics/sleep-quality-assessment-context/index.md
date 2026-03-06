# Sleep Quality Assessment Context - Sleep Health Metrics

## Overview

The Sleep Quality Assessment Context is responsible for computing standardized sleep quality metrics from scored data, administering and scoring validated patient-reported outcome instruments, performing longitudinal trend analysis, and providing clinically interpretable quality indicators. This context transforms raw scored data from the Sleep Stage Analysis Context into actionable clinical metrics.

## Scope and Boundaries

This context consumes scored epoch data, sleep architecture summaries, arousal indices, and respiratory event summaries from the Sleep Stage Analysis Context. It also consumes sleep diary data from the Sleep Data Collection Context and circadian phase data from the Circadian Rhythm Context. It produces quality metrics and composite scores consumed by the Intervention Tracking Context (for efficacy evaluation) and the Clinical Integration Context (for reporting).

This context does not perform sleep staging (that belongs upstream) and does not manage interventions (that belongs in the Intervention Tracking Context).

## Objective Sleep Quality Metrics

### Sleep Efficiency

Sleep efficiency is the ratio of total sleep time to time in bed, expressed as a percentage. Normal sleep efficiency in healthy adults is typically 85 percent or higher. Sleep efficiency below 85 percent is a criterion for insomnia diagnosis in the ICSD-3. The metric is computed from scored epoch data: total sleep time (sum of all non-wake epochs times 30 seconds) divided by time in bed (lights-out to out-of-bed).

### Sleep Onset Latency

Sleep onset latency measures the time from lights-out to the first epoch scored as any sleep stage. Normal latency is typically 10-20 minutes. Latency exceeding 30 minutes is clinically significant and may indicate sleep-onset insomnia. Latency below 5 minutes may suggest sleep deprivation or excessive daytime sleepiness.

### REM Latency

REM latency measures the time from sleep onset to the first epoch of REM sleep. Normal REM latency is 70-120 minutes. Shortened REM latency (below 15 minutes) may indicate narcolepsy or REM-rebound from prior REM suppression. Prolonged REM latency may reflect depression-related sleep disturbance.

### Wake After Sleep Onset (WASO)

WASO quantifies the total duration of wakefulness between initial sleep onset and final awakening. Elevated WASO (above 30 minutes) indicates sleep maintenance difficulty and is a criterion for insomnia diagnosis. WASO captures sleep fragmentation that sleep efficiency alone may understate.

### Total Sleep Time (TST)

TST is the cumulative duration of all epochs scored as sleep within the recording period. Recommended adult TST ranges from 7 to 9 hours per the National Sleep Foundation. Both short sleep (below 6 hours) and long sleep (above 9 hours) are associated with adverse health outcomes.

### Number of Awakenings

The count of distinct wake episodes occurring after initial sleep onset. Frequent awakenings contribute to subjective poor sleep quality even when total sleep time is adequate.

## Subjective Sleep Quality Instruments

### Pittsburgh Sleep Quality Index (PSQI)

The PSQI is a 19-item self-report questionnaire assessing sleep quality over the preceding month. It yields seven component scores: subjective sleep quality (0-3), sleep latency (0-3), sleep duration (0-3), habitual sleep efficiency (0-3), sleep disturbances (0-3), use of sleeping medication (0-3), and daytime dysfunction (0-3). The global score ranges from 0 to 21, with scores above 5 indicating clinically poor sleep quality (sensitivity 89.6 percent, specificity 86.5 percent per Buysse et al., 1989).

### Epworth Sleepiness Scale (ESS)

The ESS measures daytime sleepiness propensity by asking respondents to rate their likelihood of dozing in eight common situations (sitting and reading, watching television, sitting in a public place, riding as a passenger, lying down in the afternoon, sitting and talking, sitting after lunch, in a car while stopped in traffic). Each item is scored 0-3, yielding a total score of 0-24. Scores of 10 or above suggest excessive daytime sleepiness.

## Longitudinal Trend Analysis

The context performs trend analysis across multiple assessment periods to track patient progress, detect deterioration, and evaluate intervention outcomes. Trend detection uses clinically validated minimum important difference (MID) thresholds: for example, a PSQI change of 3 points or more is considered clinically meaningful. Trends are classified as improving, stable, or deteriorating based on direction and statistical significance.

## Normative Comparison

Quality metrics are compared against age-adjusted and sex-adjusted normative reference ranges derived from population studies. A 70-year-old patient with 80 percent sleep efficiency is evaluated differently than a 25-year-old with the same value, because normative sleep efficiency declines with age.

## Key Aggregates

The **QualityAssessment** aggregate computes and stores objective metrics for an assessment period. The **SubjectiveAssessment** aggregate captures and scores patient-reported outcome instruments.

## Domain Events Published

- **QualityMetricsCalculated**: Emitted when objective metrics are computed.
- **SubjectiveAssessmentScored**: Emitted when a PSQI or ESS is scored.
- **SleepQualityTrendDetected**: Emitted when longitudinal analysis detects a significant trend.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Buysse, D. J. et al. (1989). The Pittsburgh Sleep Quality Index. *Psychiatry Research*, 28(2), 193-213.
- Johns, M. W. (1991). A New Method for Measuring Daytime Sleepiness. *Sleep*, 14(6), 540-545.
- Ohayon, M. M. et al. (2017). National Sleep Foundation's Sleep Quality Recommendations. *Sleep Health*, 3(1), 6-19.
