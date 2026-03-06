# Intervention Tracking Context - Sleep Health Metrics

## Overview

The Intervention Tracking Context manages therapeutic interventions for sleep disorders, including behavioral therapies, pharmacological treatments, device-based therapies, and sleep hygiene programs. It tracks treatment plans, monitors adherence, evaluates outcomes against baseline metrics, and supports clinical decision-making about treatment adjustments. This context consumes quality metrics from the Sleep Quality Assessment Context and circadian data from the Circadian Rhythm Context to assess intervention efficacy.

## Scope and Boundaries

This context owns the lifecycle of treatment plans and their component interventions. It does not own sleep data collection, sleep staging, quality metric computation, or circadian phase assessment -- those belong to their respective bounded contexts. The Intervention Tracking Context consumes quality metrics and circadian data as inputs for efficacy evaluation and publishes intervention events that the Clinical Integration Context uses for reporting.

## Cognitive Behavioral Therapy for Insomnia (CBT-I)

CBT-I is the first-line treatment for chronic insomnia, recommended by the American Academy of Sleep Medicine and the American College of Physicians. The context models CBT-I as a structured multi-session protocol with the following component modules:

- **Stimulus Control**: Rules to reassociate the bed and bedroom with sleep (go to bed only when sleepy, leave bed if unable to sleep within 15-20 minutes, use the bed only for sleep and intimacy, maintain a fixed wake time regardless of sleep duration).
- **Sleep Restriction Therapy**: Limiting time in bed to match actual total sleep time (creating mild sleep deprivation to consolidate sleep), then gradually increasing time in bed as sleep efficiency improves beyond 85-90 percent.
- **Cognitive Restructuring**: Identifying and modifying dysfunctional beliefs about sleep (catastrophizing about consequences of poor sleep, unrealistic sleep expectations, misattribution of daytime impairments).
- **Relaxation Training**: Progressive muscle relaxation, diaphragmatic breathing, guided imagery, and autogenic training to reduce somatic and cognitive hyperarousal.
- **Sleep Hygiene Education**: Behavioral and environmental recommendations as an adjunctive component (not effective as standalone treatment).

The context tracks session scheduling, homework completion, sleep diary compliance, and module progression. It evaluates CBT-I outcome using pre-post comparison of sleep efficiency, sleep latency, WASO, PSQI global score, and Insomnia Severity Index (ISI). Remission is defined as ISI below 8 and PSQI at or below 5.

## Medication Tracking

The context models pharmacological interventions for sleep disorders:

- **Benzodiazepine receptor agonists**: Zolpidem, zaleplon, eszopiclone -- short-term treatment with tracking of dose, timing, and side effects.
- **Melatonin receptor agonists**: Ramelteon, tasimelteon -- chronobiological agents for sleep onset and non-24-hour sleep-wake disorder.
- **Orexin receptor antagonists**: Suvorexant, lemborexant -- dual orexin receptor antagonists for insomnia.
- **Melatonin supplements**: Exogenous melatonin at various doses and timing for circadian rhythm disorders.
- **Other agents**: Trazodone, doxepin, gabapentin -- off-label use for insomnia with tracking of clinical rationale.

The context tracks medication name, dose, timing relative to bedtime, start date, duration, side effects, and clinical response. It flags potential interactions between multiple concurrent medications and between medications and device therapy.

## Device Therapy (CPAP/BiPAP)

The context manages positive airway pressure therapy for obstructive sleep apnea:

- **Device assignment**: CPAP, auto-CPAP (APAP), or bilevel positive airway pressure (BiPAP) with prescribed pressure settings.
- **Mask interface**: Nasal mask, nasal pillows, full-face mask, or oral mask -- tracking fit, comfort, and leak issues.
- **Adherence monitoring**: Nightly usage hours, percentage of nights used, average usage per night, mask leak rates, residual AHI under therapy, and pressure delivery data.
- **Compliance criteria**: CMS defines CPAP compliance as usage of at least 4 hours per night on at least 70 percent of nights in a consecutive 30-day period within the first 90 days. Non-compliance may result in loss of insurance coverage for the device.

The context tracks device settings adjustments, mask changes, and correlates adherence data with quality metric changes to assess therapeutic benefit.

## Sleep Hygiene Programs

The context models structured sleep hygiene interventions as a set of behavioral and environmental recommendations:

- Maintain a consistent sleep-wake schedule, including weekends.
- Create an optimal sleep environment (dark, quiet, cool).
- Avoid stimulants (caffeine, nicotine) within 6 hours of bedtime.
- Avoid alcohol within 3 hours of bedtime.
- Engage in regular physical activity, but not within 2 hours of bedtime.
- Limit screen exposure and blue light in the evening.
- Establish a relaxing pre-sleep routine.

The context tracks patient engagement with each recommendation and correlates compliance with quality metric outcomes.

## Key Aggregates

The **InterventionPlan** aggregate represents the comprehensive treatment plan containing one or more InterventionModule entities. The **AdherenceRecord** aggregate tracks device therapy usage over defined periods.

## Domain Events Published

- **InterventionStarted**: Emitted when a new intervention module begins.
- **AdherenceReported**: Emitted when device adherence data is processed.
- **InterventionOutcomeAssessed**: Emitted when treatment efficacy is evaluated.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Edinger, J. D. et al. (2021). Behavioral and Psychological Treatments for Chronic Insomnia Disorder in Adults: An AASM Systematic Review and Meta-Analysis. *Journal of Clinical Sleep Medicine*, 17(2), 255-262.
- Qaseem, A. et al. (2016). Management of Chronic Insomnia Disorder in Adults: A Clinical Practice Guideline from the ACP. *Annals of Internal Medicine*, 165(2), 125-133.
- Patil, S. P. et al. (2019). Treatment of Adult Obstructive Sleep Apnea with Positive Airway Pressure: An AASM Clinical Practice Guideline. *Journal of Clinical Sleep Medicine*, 15(2), 335-343.
