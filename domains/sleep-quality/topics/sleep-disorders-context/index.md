# Sleep Disorders Context

## Overview

The Sleep Disorders Context manages the identification, classification, and clinical tracking of sleep disorders according to the International Classification of Sleep Disorders, Third Edition (ICSD-3). This context provides the diagnostic framework that connects assessment data to treatment planning. It models the lifecycle of disorder diagnoses from initial suspicion through confirmation, treatment, and resolution, maintaining clinical accuracy and supporting evidence-based clinical decision-making.

## Core Responsibilities

This context owns the domain logic for sleep disorder classification and diagnostic tracking. It maintains the taxonomy of sleep disorders aligned with ICSD-3 categories. It evaluates whether assessment data meets diagnostic criteria for specific disorders. It tracks diagnosis status over time as treatment progresses and clinical conditions evolve. It publishes diagnostic events that trigger treatment eligibility in the Behavioral Interventions Context and clinical milestone recording in the Outcomes Tracking Context.

## Disorder Classifications

### Insomnia Disorder

Insomnia disorder is characterized by difficulty initiating sleep, difficulty maintaining sleep, or early morning awakening, occurring at least three nights per week for at least three months, despite adequate opportunity for sleep, and causing clinically significant distress or impairment in functioning. The context models subtypes: sleep onset insomnia (prolonged sleep onset latency), sleep maintenance insomnia (elevated WASO), early morning awakening insomnia, and mixed presentations.

### Obstructive Sleep Apnea (OSA)

OSA involves repeated upper airway obstruction during sleep, causing apneas and hypopneas. The context models severity classification based on the Apnea-Hypopnea Index: mild (AHI 5-14), moderate (AHI 15-29), and severe (AHI 30 or greater). Diagnosis requires either AHI of 15 or greater, or AHI of 5 or greater with associated symptoms (excessive daytime sleepiness, unrefreshing sleep, witnessed apneas). The context tracks positional and REM-related variants.

### Restless Legs Syndrome (RLS)

RLS is a sensorimotor disorder characterized by an urge to move the legs, usually accompanied by uncomfortable sensations, that begins or worsens during periods of rest, is partially or totally relieved by movement, and occurs exclusively or predominantly in the evening or night. The context models the four essential diagnostic criteria, severity assessment (using the International RLS Severity Scale), and associated periodic limb movement disorder (PLMD).

### Narcolepsy

Narcolepsy involves excessive daytime sleepiness and abnormal REM sleep manifestations. The context distinguishes Type 1 narcolepsy (with cataplexy and/or CSF hypocretin-1 deficiency) from Type 2 narcolepsy (without cataplexy, normal CSF hypocretin-1). Diagnostic criteria include mean sleep latency of 8 minutes or less and two or more sleep-onset REM periods on the Multiple Sleep Latency Test.

### Parasomnias

Parasomnias are undesirable physical or experiential events that accompany sleep. The context models NREM-related parasomnias (sleepwalking, sleep terrors, confusional arousals), REM-related parasomnias (REM sleep behavior disorder, nightmare disorder), and other parasomnias (sleep enuresis, sleep-related eating disorder). Each parasomnia type has its own diagnostic criteria and clinical tracking requirements.

## Key Aggregates

### Disorder Diagnosis

The primary aggregate maintains the diagnosis lifecycle. The root entity tracks disorder type, ICSD-3 classification code, severity, diagnostic criteria fulfillment, diagnosing clinician, diagnosis date, and current status (suspected, confirmed, in treatment, in remission, resolved). The aggregate enforces that a diagnosis cannot be confirmed unless the minimum required criteria are satisfied.

### Clinical History

A supporting aggregate maintains the clinical history relevant to sleep disorder evaluation, including prior diagnoses, family history, medication history, and comorbid conditions that influence sleep disorder presentation and treatment selection.

## Domain Events Published

The context publishes DisorderDiagnosed when a diagnosis is confirmed, DisorderSeverityChanged when severity reclassification occurs based on new clinical data, and DiagnosisResolved when a disorder is clinically resolved. These events carry the clinical detail necessary for downstream contexts to adjust treatment protocols and update outcome records.

## Relationships with Other Contexts

The Sleep Disorders Context acts as a customer of the Sleep Assessment Context, consuming assessment results for diagnostic evaluation. It acts as a supplier to the Behavioral Interventions Context, providing diagnosis information that determines treatment eligibility and protocol selection. It publishes clinical milestones consumed by the Outcomes Tracking Context. It maintains an anti-corruption layer at its boundary with external EHR systems that use ICD-10 coding.

## Diagnostic Reasoning

The context supports structured diagnostic reasoning by modeling the evaluation of individual diagnostic criteria against available clinical evidence. For each potential diagnosis, the context tracks which criteria have sufficient evidence, which criteria lack evidence, and which criteria have contradictory evidence. This structured approach supports clinical decision-making without replacing clinical judgment.

## Comorbidity Handling

Sleep disorders frequently co-occur. Insomnia is comorbid with OSA in many patients. Depression and anxiety commonly accompany insomnia. The context supports multiple concurrent active diagnoses for a single patient and models the relationships between comorbid conditions, including how one disorder may exacerbate or mask another.

## Standards Alignment

The context aligns its disorder taxonomy with ICSD-3, maintained by the American Academy of Sleep Medicine. ICD-10-CM codes are maintained as secondary classifications for interoperability with external clinical and billing systems, with translation handled by the anti-corruption layer.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- American Academy of Sleep Medicine. (2014). International Classification of Sleep Disorders, 3rd Edition (ICSD-3).
- Sateia, M.J. (2014). International Classification of Sleep Disorders, Third Edition. Chest, 146(5), 1387-1394.
