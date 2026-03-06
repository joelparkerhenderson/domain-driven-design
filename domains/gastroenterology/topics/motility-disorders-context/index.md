# Motility Disorders Context

## Overview

The Motility Disorders Context manages the evaluation, diagnosis, and treatment of
gastrointestinal motility disorders and functional gastrointestinal disorders. This
context handles esophageal and anorectal manometry studies, ambulatory pH and
impedance monitoring, gastroesophageal reflux disease (GERD) management, and the
classification of functional GI disorders according to Rome IV criteria. Motility
assessment relies on objective measurements interpreted through standardized
diagnostic frameworks.

## Scope and Responsibilities

This context is responsible for:

- Performing and interpreting esophageal high-resolution manometry studies.
- Performing and interpreting anorectal manometry studies.
- Conducting ambulatory pH monitoring and impedance-pH monitoring studies.
- Applying the Chicago Classification to esophageal manometry data.
- Calculating DeMeester scores and acid exposure metrics from pH studies.
- Classifying functional GI disorders using Rome IV diagnostic criteria.
- Managing GERD treatment protocols including step-up and step-down approaches.
- Evaluating dysphagia, non-cardiac chest pain, and refractory reflux symptoms.
- Differentiating structural from functional causes of GI symptoms.

## Core Aggregates

### ManometryStudy

Represents a complete manometry study session with all raw measurements, calculated
parameters, and diagnostic interpretation. For esophageal manometry, this includes
lower esophageal sphincter (LES) pressure measurements, integrated relaxation
pressure (IRP), distal contractile integral (DCI), distal latency, and peristaltic
pattern analysis. For anorectal manometry, this includes resting and squeeze pressures,
rectoanal inhibitory reflex, and balloon expulsion test results.

Key invariants: Chicago Classification diagnosis must be consistent with the measured
manometric parameters. All standard test swallows must be documented and analyzed.
Classification must use the most current version of the Chicago Classification.

### PhMonitoringSession

Represents an ambulatory pH or impedance-pH monitoring study. Contains total recording
time, upright and supine acid exposure percentages, DeMeester score, total number of
reflux events, symptom-reflux correlation indices (symptom index, symptom sensitivity
index, symptom association probability), and diagnostic interpretation.

Key invariant: the DeMeester score must be correctly calculated from the six standard
components. Symptom correlation indices require a minimum number of symptom events to
be statistically valid.

### MotilityDiagnosis

Represents the diagnostic conclusion for a patient's motility or functional GI
disorder. Contains the Rome IV classification (if applicable), Chicago Classification
(for esophageal motility), supporting study data, and treatment plan. The diagnosis
may evolve as additional data becomes available or as symptoms change over time.

Key invariant: Rome IV diagnoses must satisfy the specified symptom duration and
frequency criteria. The diagnosis must document which specific criteria were met.

## Key Entities

- StudySwallow: an individual test swallow during manometry with associated pressure
  measurements and pattern classification.
- RefluxEvent: a single reflux episode detected during pH monitoring with timestamp,
  duration, proximal extent, and composition (acid, weakly acid, non-acid).
- SymptomEntry: a patient-reported symptom during pH monitoring with timestamp for
  symptom-reflux correlation analysis.

## Key Value Objects

- IntegratedRelaxationPressure: the median of the four-second lowest LES pressure
  during the deglutitive window, in mmHg.
- DistalContractileIntegral: the vigor of esophageal peristalsis measured in mmHg-cm-s.
- DeMeesterScore: composite acid reflux score calculated from six pH parameters.
- AcidExposureTime: percentage of total recording time with esophageal pH below 4.0.
- ChicagoClassification: the esophageal motility disorder diagnosis per Chicago
  Classification criteria.
- RomeIVDiagnosis: the specific functional GI disorder classification.
- SymptomAssociationProbability: statistical measure of symptom-reflux correlation.

## Domain Events

- MotilityStudyCompleted: signals completion of manometry or pH study with results.
- FunctionalDiagnosisEstablished: signals Rome IV criteria satisfaction.
- GERDDiagnosisConfirmed: signals objective confirmation of pathologic reflux.
- TreatmentEscalationIndicated: signals need for therapy adjustment based on study results.

## Clinical Workflows

### Esophageal Manometry Workflow

Patients with dysphagia, non-cardiac chest pain, or pre-operative evaluation for
anti-reflux surgery undergo esophageal manometry. The ManometryStudy aggregate captures
all test swallows, the ChicagoClassificationService applies the diagnostic algorithm,
and the resulting diagnosis informs treatment decisions.

### pH Monitoring Workflow

Patients with refractory GERD symptoms, atypical reflux presentations, or
pre-operative reflux evaluation undergo ambulatory pH monitoring. The PhMonitoringSession
aggregate captures acid exposure data and symptom events. DeMeester score and symptom
correlation indices are calculated to determine if pathologic reflux is present and
if symptoms correlate with reflux events.

### Functional Disorder Evaluation Workflow

Patients with chronic GI symptoms without structural explanation are evaluated against
Rome IV criteria. The RomeIVClassificationService assesses symptom type, duration,
frequency, and onset timing. When criteria are satisfied, the MotilityDiagnosis
aggregate is created with the specific Rome IV classification.

### GERD Management Workflow

GERD treatment follows a structured protocol: lifestyle modification, step-up PPI
therapy, and if refractory, pH monitoring to guide further management. The context
tracks treatment response and determines when objective testing is indicated.

## Integration Points

- Upstream: receives referrals from Clinical Assessment Context for motility evaluation.
- Downstream: provides diagnostic conclusions to Clinical Assessment for treatment planning.
- Cross-context: coordinates with Endoscopic Procedures for combined evaluation (endoscopy
  with manometry or pH study).

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Yadlapati, R. et al. (2021). "Chicago Classification v4.0." Neurogastroenterology and Motility.
- Drossman, D.A. (2016). "Functional GI Disorders: Rome IV." Gastroenterology.
- Gyawali, C.P. et al. (2018). "Lyon Consensus on GERD diagnosis." Gut.
