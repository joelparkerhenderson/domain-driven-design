# Sleep Health Metrics Standards - Sleep Health Metrics

## Overview

The sleep health metrics domain operates within a framework of clinical, technical, and interoperability standards that inform domain model design, value object definitions, scoring rules, and data exchange formats. These standards provide the authoritative basis for the ubiquitous language and ensure that the domain model produces clinically valid and interoperable outputs.

## Clinical Scoring Standards

### AASM Manual for the Scoring of Sleep and Associated Events

The American Academy of Sleep Medicine (AASM) scoring manual is the authoritative standard for sleep stage scoring in North America and is widely adopted internationally. Currently in version 2.6 (2020), it defines:

- **Epoch duration**: 30 seconds as the standard scoring window.
- **Sleep stage definitions**: EEG, EOG, and EMG criteria for Wake, N1, N2, N3, and REM stages.
- **Arousal scoring**: Minimum 3-second EEG frequency shift with 10 seconds of preceding stable sleep.
- **Respiratory event scoring**: Apnea (10-second cessation of airflow), hypopnea (30 percent reduction with 3 percent desaturation or arousal), RERA definitions.
- **Periodic limb movement scoring**: Duration, interval, and series requirements.
- **Cardiac event scoring**: Criteria for arrhythmia detection during sleep.
- **Technical specifications**: Recommended montage, filter settings, sampling rates, and electrode placement.

The domain model's Sleep Stage Analysis Context implements these rules directly. ScoredEpoch value objects reference the AASM version used for scoring, enabling traceability and version migration.

### Rechtschaffen and Kales (R&K) Manual

The original 1968 R&K scoring manual preceded the AASM manual and used a different stage classification (Stages 1, 2, 3, 4, and REM). While superseded by AASM scoring in clinical practice, the domain model supports R&K-scored historical data through translation mappings (R&K Stage 3 and Stage 4 map to AASM N3). This enables longitudinal analysis spanning the standard transition period.

## Diagnostic Classification Standards

### ICSD-3 (International Classification of Sleep Disorders, 3rd Edition)

Published by the AASM in 2014, the ICSD-3 provides the diagnostic taxonomy for sleep medicine. It classifies sleep disorders into major categories:

- **Insomnia disorders**: Chronic insomnia disorder, short-term insomnia disorder.
- **Sleep-related breathing disorders**: Obstructive sleep apnea, central sleep apnea syndromes, sleep-related hypoventilation disorders.
- **Central disorders of hypersomnolence**: Narcolepsy type 1 and type 2, idiopathic hypersomnia.
- **Circadian rhythm sleep-wake disorders**: Delayed sleep-wake phase disorder, advanced sleep-wake phase disorder, non-24-hour sleep-wake rhythm disorder, shift work disorder.
- **Parasomnias**: NREM-related (sleepwalking, sleep terrors), REM-related (REM sleep behavior disorder).
- **Sleep-related movement disorders**: Restless legs syndrome, periodic limb movement disorder.

The domain model uses ICSD-3 codes alongside ICD-10-CM codes for condition classification in the Clinical Integration Context.

### ICD-10-CM Sleep Disorder Codes

The International Classification of Diseases, 10th Revision, Clinical Modification provides the billing and administrative coding system. Key sleep disorder codes include G47.0 (insomnia), G47.30 (sleep apnea, unspecified), G47.33 (obstructive sleep apnea), G47.20 (circadian rhythm sleep disorder, unspecified), and G47.411 (narcolepsy with cataplexy). The domain model maps ICSD-3 diagnostic categories to ICD-10-CM codes for clinical integration.

## Assessment Instrument Standards

### Pittsburgh Sleep Quality Index (PSQI)

Developed by Buysse et al. (1989), the PSQI is a validated self-report instrument with established psychometric properties. The domain model implements the standard PSQI scoring algorithm exactly as published, including the seven component scores and global score computation. The clinical cutoff of 5 for poor sleep quality has sensitivity of 89.6 percent and specificity of 86.5 percent.

### Epworth Sleepiness Scale (ESS)

Developed by Johns (1991), the ESS is a validated measure of daytime sleepiness. The domain model implements the standard 8-item scoring algorithm. The clinical cutoff of 10 for excessive daytime sleepiness is used for clinical interpretation.

### Insomnia Severity Index (ISI)

Developed by Morin (1993), the ISI is a 7-item self-report measure of insomnia severity. Scores range from 0 to 28: 0-7 (no clinically significant insomnia), 8-14 (subthreshold), 15-21 (moderate), 22-28 (severe).

## Interoperability Standards

### HL7 FHIR R4

Fast Healthcare Interoperability Resources Release 4 (2019) provides the data exchange standard for clinical integration. Key FHIR resources used in the domain include:

- **Observation**: For individual sleep metrics (AHI, sleep efficiency, TST) with LOINC coding.
- **DiagnosticReport**: For sleep study reports referencing component observations.
- **Condition**: For sleep disorder diagnoses with ICSD-3 and ICD-10-CM coding.
- **CarePlan**: For intervention plans (CBT-I protocols, CPAP therapy).
- **DeviceUseStatement**: For CPAP usage tracking.
- **QuestionnaireResponse**: For PSQI, ESS, and ISI raw responses.

### LOINC Codes for Sleep Medicine

Logical Observation Identifiers Names and Codes provide standardized identifiers for clinical measurements. Relevant LOINC codes include sleep duration, sleep efficiency, AHI, oxygen desaturation index, PSQI total score, and ESS total score. The domain model uses LOINC codes in FHIR Observation resources to ensure semantic interoperability.

### SNOMED CT

Systematized Nomenclature of Medicine Clinical Terms provides concept identifiers for clinical findings, procedures, and disorders. Sleep-related SNOMED CT concepts complement ICD-10-CM coding with finer granularity.

## Data Format Standards

### EDF/EDF+

European Data Format (EDF) and its extension EDF+ are the standard file formats for polysomnographic signal data. The Sleep Data Collection Context's anti-corruption layer parses these formats during data ingestion.

## References

- Berry, R. B. et al. (2020). *AASM Manual for the Scoring of Sleep and Associated Events*. Version 2.6. AASM.
- American Academy of Sleep Medicine. (2014). *International Classification of Sleep Disorders*, 3rd Edition.
- Buysse, D. J. et al. (1989). The Pittsburgh Sleep Quality Index. *Psychiatry Research*, 28(2), 193-213.
- Johns, M. W. (1991). The Epworth Sleepiness Scale. *Sleep*, 14(6), 540-545.
- Morin, C. M. (1993). *Insomnia: Psychological Assessment and Management*. Guilford Press.
- HL7 International. (2019). *FHIR R4 Specification*. https://hl7.org/fhir/R4/
- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
