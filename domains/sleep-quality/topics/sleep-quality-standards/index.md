# Sleep Quality Standards in the Sleep Quality Domain

## Overview

Domain-specific standards inform value object design, published languages, and clinical validation rules throughout the sleep quality domain. Sleep medicine has a rich set of professional standards, validated instruments, classification systems, and practice guidelines established by organizations such as the American Academy of Sleep Medicine (AASM), the International RLS Study Group, and various regulatory bodies. Incorporating these standards into the domain model ensures clinical accuracy, interoperability, and adherence to evidence-based practice.

## International Classification of Sleep Disorders (ICSD-3)

The ICSD-3, published by the American Academy of Sleep Medicine in 2014, is the authoritative taxonomy for sleep disorder classification. It organizes sleep disorders into seven major categories: insomnia, sleep-related breathing disorders, central disorders of hypersomnolence, circadian rhythm sleep-wake disorders, parasomnias, sleep-related movement disorders, and other sleep disorders. The Sleep Disorders Context uses ICSD-3 as its primary classification framework, modeling each disorder category and its subtypes with the diagnostic criteria specified in the standard.

## AASM Scoring Manual

The AASM Manual for the Scoring of Sleep and Associated Events provides standardized rules for scoring polysomnography recordings. It defines the criteria for identifying sleep stages (N1, N2, N3, REM), respiratory events (apneas, hypopneas, respiratory effort-related arousals), movement events (periodic limb movements), and arousals. The Sleep Assessment Context implements these scoring rules as domain logic, ensuring that polysomnography data interpretation is consistent with the accepted standard.

## Pittsburgh Sleep Quality Index (PSQI)

The PSQI, developed by Buysse et al. (1989), is the most widely used self-report measure of sleep quality. The standard defines 19 self-rated questions yielding seven component scores and a global score. The scoring algorithm is precisely specified and is implemented as domain logic in the Assessment Scoring Service. The standard's cutoff of 5 for distinguishing good from poor sleepers (sensitivity 89.6 percent, specificity 86.5 percent) is modeled as a value object classification threshold.

## Epworth Sleepiness Scale (ESS)

The ESS, developed by Johns (1991), measures general daytime sleepiness through eight situational items. The standard defines the situations, the 0-3 rating scale, and the interpretation of total scores. The domain model implements the scoring algorithm and the classification scheme (0-10 normal, 11-14 mild, 15-17 moderate, 18-24 severe excessive daytime sleepiness) as defined by the standard.

## Insomnia Severity Index (ISI)

The ISI, developed by Bastien et al. (2001), measures insomnia severity through seven items rated on a 0-4 scale. The standard defines scoring (total range 0-28) and clinical interpretation (0-7 no clinically significant insomnia, 8-14 subthreshold, 15-21 moderate, 22-28 severe). The domain model implements these classifications as value object thresholds.

## CPAP Compliance Standards

Medicare and most insurance providers define CPAP compliance as usage of 4 or more hours per night on 70 percent or more of nights during a consecutive 30-day period within the first 90 days of use. This compliance standard is modeled in the Outcomes Tracking Context as a milestone definition and in the Device Integration Context as a compliance calculation rule. The standard directly influences how CPAP therapy data is aggregated and reported.

## Sleep Diary Consensus Standards

The Consensus Sleep Diary, developed by Carney et al. (2012), standardizes the items and format for self-report sleep diaries used in clinical and research settings. Core items include the time the individual got into bed, the time they tried to go to sleep, sleep onset latency, number of awakenings, duration of awakenings (WASO), final wake time, time out of bed, and sleep quality rating. The Sleep Assessment Context implements the Consensus Sleep Diary format as its standard diary template.

## Practice Guidelines

The American Academy of Sleep Medicine publishes clinical practice guidelines for the treatment of sleep disorders. The 2017 guideline for insomnia recommends CBT-I as first-line treatment for chronic insomnia. The 2019 guideline for OSA provides recommendations for positive airway pressure therapy. These guidelines inform the Behavioral Interventions Context's protocol design and the Sleep Disorders Context's treatment pathway modeling.

## Standard Units and Measurements

The domain enforces standard measurement units. Time durations are represented in minutes for sleep metrics (SOL, WASO, TST). Light is measured in lux. Noise is measured in decibels (dB). Temperature supports both Celsius and Fahrenheit with explicit unit marking. AHI is expressed in events per hour. These standardized units are enforced at the value object level, preventing unit confusion.

## Interoperability Standards

For external system integration, the domain references HL7 FHIR (Fast Healthcare Interoperability Resources) for clinical data exchange and ICD-10-CM for diagnostic code mapping. These interoperability standards are handled by anti-corruption layers at the domain boundary, not incorporated directly into the domain model.

## Standards Governance

Standards evolve as research advances. The ICSD is periodically revised (currently on its third edition, with ICSD-3-TR published in 2023). Instrument scoring may be updated as new validation studies are published. The domain model treats standards-derived logic as configurable policy rather than hard-coded rules, allowing updates to be applied without structural model changes.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- American Academy of Sleep Medicine. (2014). International Classification of Sleep Disorders, 3rd Edition (ICSD-3).
- Berry, R.B. et al. (2017). AASM Manual for the Scoring of Sleep and Associated Events. Version 2.4.
- Buysse, D.J. et al. (1989). The Pittsburgh Sleep Quality Index. Psychiatry Research, 28(2), 193-213.
- Carney, C.E. et al. (2012). The Consensus Sleep Diary. Sleep, 35(2), 287-302.
- Edinger, J.D. et al. (2021). Behavioral and Psychological Treatments for Chronic Insomnia Disorder in Adults. Journal of Clinical Sleep Medicine, 17(2), 255-262.
