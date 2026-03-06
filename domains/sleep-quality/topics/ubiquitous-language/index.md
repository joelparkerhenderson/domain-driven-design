# Ubiquitous Language in the Sleep Quality Domain

## Overview

Ubiquitous language is the shared vocabulary used consistently by domain experts, developers, and stakeholders when discussing the sleep quality domain. This language bridges the gap between clinical sleep medicine terminology and software modeling concepts, ensuring that conversations, documentation, and code all use the same terms with the same meanings. The ubiquitous language eliminates ambiguity and reduces the risk of misunderstanding between technical and clinical team members.

## Purpose

In the sleep quality domain, ubiquitous language is particularly important because the domain spans multiple professional disciplines. Sleep medicine physicians, behavioral psychologists, device engineers, data scientists, and software developers must communicate precisely about concepts that may have different colloquial meanings. By establishing and maintaining a ubiquitous language, the team ensures that a term like "sleep efficiency" always refers to the ratio of total sleep time to time in bed, expressed as a percentage.

## Core Assessment Terms

Sleep Onset Latency refers specifically to the time from lights-out to the first epoch of any sleep stage, measured in minutes. This term is used consistently across the Sleep Assessment Context and the Outcomes Tracking Context. Wake After Sleep Onset (WASO) refers to the cumulative minutes of wakefulness after initial sleep onset, excluding the final awakening. Total Sleep Time refers to the sum of all sleep epochs within a recording period.

## Clinical Instrument Terms

The Pittsburgh Sleep Quality Index (PSQI) refers to the 19-item self-report questionnaire developed by Buysse et al. (1989) that yields seven component scores and a global score ranging from 0 to 21, where scores above 5 indicate poor sleep quality. The Epworth Sleepiness Scale (ESS) refers to the 8-item questionnaire developed by Johns (1991) that measures general daytime sleepiness on a scale of 0 to 24. These instrument names are never abbreviated without first establishing the full name in context.

## Disorder Classification Terms

The domain uses ICSD-3 terminology for disorder classification. Insomnia Disorder (not simply "insomnia") refers to the clinical condition meeting ICSD-3 diagnostic criteria. Obstructive Sleep Apnea uses the standard abbreviation OSA after first use. The Apnea-Hypopnea Index (AHI) is always described with its unit: events per hour of sleep.

## Behavioral Intervention Terms

Cognitive Behavioral Therapy for Insomnia (CBT-I) refers to the multi-component structured treatment program, not a single technique. Sleep Restriction Therapy is a specific protocol within CBT-I that prescribes a time-in-bed window based on average total sleep time. Stimulus Control refers to the set of instructions that strengthen the bed-sleep association. These terms are never used interchangeably.

## Device and Measurement Terms

Actigraphy refers to wrist-worn accelerometer-based monitoring and its algorithmic sleep-wake estimation, distinct from polysomnography. Polysomnography (PSG) refers to the comprehensive attended laboratory sleep study. CPAP refers to the continuous positive airway pressure device, and its data (including AHI, leak rate, and usage hours) has specific meaning within the Device Integration Context.

## Environment Terms

Sleep Environment refers to the totality of physical conditions in the sleeping space. Light Level is measured in lux. Ambient Noise is measured in decibels (dB). Temperature is recorded in both Celsius and Fahrenheit with explicit unit marking. These measurement terms carry precise physical definitions that must not be loosely approximated.

## Outcomes Terms

Sleep Quality Score is a composite metric defined within the Outcomes Tracking Context, distinct from any single instrument score. Daytime Functioning encompasses cognitive performance, mood state, energy level, and social functioning. Outcomes Trend refers to a time-series analysis of repeated measurements, not a single comparison.

## Language Governance

The ubiquitous language is maintained as a living glossary that is updated whenever new terms emerge or existing definitions are refined. All team members are responsible for using the language consistently in conversation, documentation, code (including class names, method names, and variable names), and user-facing content. Disagreements about terminology are resolved through collaborative sessions with domain experts.

## Boundary-Specific Variations

While the ubiquitous language provides a domain-wide foundation, each bounded context may refine or extend terms for internal use. These refinements must be documented in the context-specific documentation and must not contradict the domain-wide definitions. When a term has context-specific meaning, the context name should be used as a qualifier in cross-context communication.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 2 on ubiquitous language.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 1 on developing a ubiquitous language.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 2 on discovering domain knowledge.
- Buysse, D.J. et al. (1989). The Pittsburgh Sleep Quality Index. Psychiatry Research, 28(2), 193-213.
- Johns, M.W. (1991). A New Method for Measuring Daytime Sleepiness: The Epworth Sleepiness Scale. Sleep, 14(6), 540-545.
