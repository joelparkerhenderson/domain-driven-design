# Sleep Disorders Context - Otolaryngology Domain

## Overview

The Sleep Disorders Context manages the evaluation and treatment of sleep-disordered breathing within the otolaryngology domain. It encompasses the full care pathway from initial screening and polysomnography ordering through diagnosis, conservative management with continuous positive airway pressure (CPAP), and surgical intervention when conservative therapy fails. This context focuses on the ENT-specific aspects of sleep medicine, particularly upper airway evaluation and surgical management of obstructive sleep apnea (OSA).

## Subdomain Classification

Sleep Disorders is a supporting subdomain. It follows established diagnostic criteria from the American Academy of Sleep Medicine and treatment protocols that are well-documented in clinical guidelines. While custom modeling is needed to capture the relationship between airway anatomy and treatment selection, the underlying classification systems (AHI thresholds, Epworth Sleepiness Scale) and treatment decision trees are standardized.

## Ubiquitous Language

- **Sleep Evaluation**: The primary aggregate representing a patient's sleep disorder journey from screening through treatment outcome.
- **Polysomnography (PSG)**: An overnight sleep study recording EEG, EOG, EMG, airflow, respiratory effort, oximetry, and body position.
- **Apnea-Hypopnea Index (AHI)**: Events per hour of sleep; classifies OSA severity (normal <5, mild 5-15, moderate 15-30, severe >30).
- **CPAP Therapy**: Continuous positive airway pressure delivered via mask to maintain upper airway patency during sleep.
- **CPAP Compliance**: Adherence to prescribed CPAP therapy, typically measured as average hours of use per night over a defined period.
- **Uvulopalatopharyngoplasty (UPPP)**: Surgical removal of excess pharyngeal tissue to widen the upper airway.
- **Epworth Sleepiness Scale (ESS)**: An 8-item questionnaire scoring daytime sleepiness from 0 to 24.

## Aggregate: Sleep Evaluation

The Sleep Evaluation is the primary aggregate root, tracking a patient's sleep disorder from initial presentation through treatment outcome.

### Entities Within the Aggregate

- **Sleep Screening**: Records initial symptom assessment (ESS, STOP-BANG questionnaire), physical examination findings (BMI, neck circumference, Mallampati score, tonsil size), and decision to pursue formal sleep study.
- **Polysomnography Result**: Records full sleep study data including AHI, oxygen nadir, sleep architecture, and positional data.
- **CPAP Prescription**: Documents prescribed device type, pressure settings, mask selection, and initiation date.
- **CPAP Compliance Record**: Tracks adherence data at defined intervals (30-day, 90-day, annual).
- **Surgical Intervention Plan**: Documents surgical procedure selection, anatomical rationale, and expected outcomes for patients pursuing surgery.
- **Treatment Outcome**: Records post-treatment AHI, ESS, and patient-reported quality of life measures.

### Key Value Objects

- **Apnea-Hypopnea Index**: Total AHI, REM AHI, supine AHI, severity classification.
- **Epworth Sleepiness Scale Score**: Total score (0-24) with severity classification.
- **Oxygen Desaturation Index**: ODI value, nadir SpO2, percentage time below 90%.
- **CPAP Settings**: Mode (fixed, auto), pressure range, relief settings, ramp time.
- **Mallampati Score**: Airway classification (I, II, III, IV).
- **Friedman Staging**: Composite staging system combining tonsil size, Mallampati score, and BMI for UPPP outcome prediction.

### Invariants

- A CPAP prescription requires a documented AHI at or above the treatment threshold (AHI >= 5 with symptoms, or AHI >= 15 regardless of symptoms).
- CPAP compliance must be assessed at defined intervals after initiation.
- Surgical referral for OSA requires documented CPAP intolerance or treatment failure.
- CPAP failure is defined as inability to achieve adequate adherence (>= 4 hours per night on >= 70% of nights) after reasonable trial period and troubleshooting.
- Treatment outcome measurement requires post-treatment polysomnography or home sleep test.

## Domain Events Published

- **SleepStudyCompleted**: Publishes PSG results including AHI, severity classification, and treatment recommendation.
- **CPAPPrescribed**: Notifies the system of new CPAP initiation.
- **CPAPComplianceAssessed**: Publishes compliance metrics and whether the patient meets adherence thresholds.
- **SleepSurgeryReferred**: Notifies Surgical Management Context of a patient referred for sleep surgery after failing conservative therapy.

## Domain Events Consumed

- **EncounterCompleted** (from Clinical Assessment): Receives airway examination findings (Mallampati, tonsil size, neck circumference) from general ENT encounters.
- **SurgeryCompleted** (from Surgical Management): Receives surgical outcome information for post-operative sleep study planning.

## Domain Services

- **SleepApneaSeverityClassificationService**: Classifies OSA severity from PSG data and determines appropriate treatment pathways based on AHI, symptom burden, and comorbidities.

## Clinical Workflow

1. Patient presents with sleep complaints (snoring, witnessed apneas, daytime sleepiness).
2. Sleep screening performed (ESS, STOP-BANG, physical examination).
3. Polysomnography or home sleep test ordered based on screening results.
4. Sleep study results interpreted and OSA severity classified.
5. Treatment pathway determined (positional therapy, CPAP, oral appliance, surgery).
6. If CPAP: Prescription issued, device dispensed, mask fitted.
7. CPAP compliance assessed at 30 days, 90 days, and annually.
8. If CPAP fails: Surgical consultation initiated with referral event to Surgical Management.
9. Treatment outcome measured with post-treatment sleep study.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Sleep Medicine. *International Classification of Sleep Disorders*. 3rd ed., 2014.
- Friedman, Michael. *Sleep Apnea and Snoring: Surgical and Non-Surgical Therapy*. 2nd ed., Elsevier, 2020.
