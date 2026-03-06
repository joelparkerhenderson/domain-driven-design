# Domain Services in the Urology Domain

## Overview

A domain service is a stateless operation that encapsulates domain logic which does not naturally belong to any single entity or value object. Domain services express significant business processes or transformations that span multiple aggregates or require coordination between domain objects. In the urology domain, domain services model clinical decision algorithms, risk calculations, and cross-aggregate workflows that are central to urological care delivery.

## Domain Service Characteristics

Domain services in the urology domain are stateless: they receive inputs, perform computation or coordination, and return results without maintaining internal state between invocations. They are named using verbs or verb phrases from the ubiquitous language, making their purpose immediately clear to domain experts. Domain services belong to the domain layer and must not depend on infrastructure concerns.

## Clinical Assessment Domain Services

### DiagnosticTriageService

The DiagnosticTriageService determines the appropriate diagnostic pathway based on a patient's presenting symptoms. Given a set of symptoms (hematuria, lower urinary tract symptoms, flank pain, scrotal mass), the service applies clinical algorithms to recommend the appropriate diagnostic workup sequence. For example, microscopic hematuria in a patient over 40 triggers a full hematuria workup (cystoscopy, CT urogram, urine cytology), while lower urinary tract symptoms trigger IPSS scoring and uroflowmetry.

### SymptomSeverityCalculator

The SymptomSeverityCalculator computes composite severity scores from individual symptom assessments. It takes raw questionnaire responses and produces validated clinical scores (IPSS total and subscores, AUA Symptom Index, bother score, quality of life impact). The service enforces scoring rules defined by the validating organizations and handles missing data according to established imputation protocols.

## Surgical Management Domain Services

### SurgicalRiskAssessmentService

The SurgicalRiskAssessmentService evaluates a patient's fitness for urological surgery. It integrates comorbidity data (Charlson Comorbidity Index), anticoagulation status, anesthetic risk (ASA classification), and procedure-specific risk factors to produce a composite surgical risk profile. The service identifies modifiable risk factors and recommends pre-operative optimization steps.

### ProcedureSelectionService

The ProcedureSelectionService recommends the optimal surgical approach based on clinical parameters. For stone disease, it considers stone size, location, composition, and patient anatomy to recommend ESWL, ureteroscopy, or PCNL. For prostate surgery, it considers prostate size, cancer characteristics, and patient preferences to recommend robotic versus open approach.

## Oncologic Urology Domain Services

### RiskStratificationService

The RiskStratificationService classifies urological cancers into risk groups based on multiple clinical parameters. For prostate cancer, it applies the D'Amico classification (low, intermediate, high risk) using PSA level, Gleason score, and clinical T stage. It also computes the CAPRA score, NCCN risk group, and Memorial Sloan Kettering nomogram predictions. The service takes individual clinical parameters and returns a comprehensive risk profile that informs treatment selection.

### SurveillanceProtocolService

The SurveillanceProtocolService generates monitoring schedules based on cancer type, stage, treatment received, and institutional protocols. For post-prostatectomy surveillance, it defines PSA testing intervals and imaging triggers. For bladder cancer post-TURBT, it defines cystoscopy intervals based on risk stratification (low, intermediate, high). The service produces a chronological schedule of required tests and their timing.

### PSAKineticsCalculator

The PSAKineticsCalculator computes PSA velocity and doubling time from a series of PSA measurements. It validates that sufficient measurements and time intervals exist for reliable calculation, applies the appropriate regression model, and returns kinetic parameters with confidence intervals. The service is critical for detecting biochemical recurrence after definitive treatment.

## Stone Disease Domain Services

### MetabolicRiskAssessmentService

The MetabolicRiskAssessmentService analyzes 24-hour urine collection results and serum chemistry to identify metabolic risk factors for stone formation. It classifies each analyte against reference ranges, identifies specific metabolic abnormalities (hypercalciuria, hyperoxaluria, hypocitraturia, hyperuricosuria), computes supersaturation indices, and recommends targeted dietary and pharmacologic interventions for each identified abnormality.

### StoneRecurrencePredictionService

The StoneRecurrencePredictionService estimates the likelihood of stone recurrence based on stone composition, metabolic profile, number of prior episodes, family history, and lifestyle factors. It applies validated prediction models to produce a recurrence risk category (low, moderate, high) and recommends the intensity of metabolic monitoring and prevention efforts.

## Incontinence Management Domain Services

### TreatmentEscalationService

The TreatmentEscalationService determines when a patient should progress from one level of incontinence treatment to the next. It evaluates treatment response at each stage (behavioral therapy, pelvic floor exercises, pharmacotherapy, combination therapy, neuromodulation, surgical intervention) against predefined response criteria and recommends escalation, continuation, or alternative approaches.

### ContinenceOutcomeCalculator

The ContinenceOutcomeCalculator computes standardized outcome measures from raw clinical data. It calculates pad-free rates, pad count reductions, patient-reported outcome measure (PROM) improvements, and quality of life score changes. The service enables consistent outcome reporting across patients and treatment modalities.

## Male Reproductive Health Domain Services

### FertilityPrognosisService

The FertilityPrognosisService estimates the likelihood of natural conception or successful assisted reproduction based on male factor parameters. It integrates semen analysis results, hormonal profiles, partner age, duration of infertility, and genetic test results to classify the prognosis and recommend the appropriate level of intervention (expectant management, intrauterine insemination, in vitro fertilization, intracytoplasmic sperm injection).

### TestosteroneReplacementMonitoringService

The TestosteroneReplacementMonitoringService generates monitoring schedules and evaluates safety parameters for patients on testosterone replacement therapy. It tracks hematocrit levels (flagging values above 54%), PSA changes, lipid profiles, bone density, and symptom response. The service alerts clinicians when safety thresholds are approached or when dose adjustments may be indicated.

## Cross-Context Domain Services

Some domain services operate across bounded context boundaries and are implemented as shared services or as services within a coordinating context. These cross-context services use domain events to communicate with other contexts rather than directly accessing their aggregates.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6.
- American Urological Association. Clinical Practice Guidelines and Algorithms. https://www.auanet.org/guidelines
