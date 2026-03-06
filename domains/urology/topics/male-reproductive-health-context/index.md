# Male Reproductive Health Context

## Overview

The Male Reproductive Health Context manages the evaluation and treatment of male infertility, sexual dysfunction, and hormonal health. This bounded context owns the clinical models for semen analysis interpretation, hormonal profiling, erectile function assessment, and surgical fertility interventions. It integrates with endocrinology and reproductive medicine to deliver comprehensive male reproductive care.

## Scope

This context covers male infertility evaluation (semen analysis, hormonal profiling, genetic testing, scrotal ultrasound, testicular biopsy), surgical fertility interventions (varicocelectomy, vasectomy reversal, microsurgical testicular sperm extraction), vasectomy, erectile dysfunction assessment (IIEF questionnaire, penile duplex ultrasound, nocturnal penile tumescence testing), erectile dysfunction treatment (PDE5 inhibitors, intracavernosal injection, vacuum erection device, penile prosthesis), testosterone deficiency evaluation (morning testosterone, confirmatory testing, etiology workup), testosterone replacement therapy and monitoring, and Peyronie disease management.

## Ubiquitous Language

"Azoospermia" is the complete absence of sperm in ejaculate, classified as obstructive (normal production with blocked transport) or non-obstructive (impaired production). "Total motile sperm count" is a calculated parameter: volume times concentration times motility percentage, used to predict fertility potential. "IIEF-5" or "SHIM" is the five-item International Index of Erectile Function questionnaire for erectile dysfunction severity. "Morning testosterone" specifies the required timing for accurate testosterone measurement due to diurnal variation. "Varicocele" is a dilation of the pampiniform plexus veins graded I through III.

## Aggregates

The FertilityEvaluation aggregate root manages a complete male infertility workup. It contains SemenAnalysis value objects, HormonalProfile value objects, GeneticTestResult entities, PhysicalExamination entities, and ScrotalUltrasound entities. The ErectileDysfunctionAssessment aggregate manages ED evaluation with IIEFScore value objects, vascular studies, and treatment response records. The TestosteroneManagement aggregate tracks testosterone replacement therapy with monitoring protocols.

## Key Entities

SemenSample entities represent individual semen specimens with accession numbers, collection parameters, and WHO-referenced analysis results. TestosteroneLevel entities record individual measurements with time-of-draw validation. PenileDuplexStudy entities capture vascular flow parameters (peak systolic velocity, end-diastolic velocity, resistive index). TreatmentResponse entities track outcomes of each intervention attempted.

## Value Objects

SemenParameters captures WHO-referenced semen analysis results with abnormality classifications. IIEFScore quantifies erectile dysfunction severity on a 5-25 scale. TestosteroneProfile combines total testosterone, free testosterone, SHBG, LH, FSH, and estradiol with derived classifications (eugonadal, primary hypogonadism, secondary hypogonadism). PeyronieAssessment captures plaque location, curvature degree, and functional impact.

## Domain Events

SemenAnalysisCompleted triggers further workup if abnormal (hormonal evaluation, genetic testing, scrotal ultrasound). TestosteroneDeficiencyConfirmed initiates treatment planning for testosterone replacement. ErectileDysfunctionClassified determines the appropriate treatment pathway. FertilityInterventionRequested sends a referral to the Surgical Management Context for varicocelectomy, vasectomy reversal, or microsurgical sperm extraction. VasectomyConfirmed records successful post-vasectomy semen analysis showing azoospermia.

## Infertility Evaluation Workflow

The infertility evaluation follows a structured pathway. Initial assessment includes history, physical examination, and at least two semen analyses separated by a minimum interval. If semen analysis is abnormal, hormonal evaluation (FSH, LH, testosterone, prolactin) is performed. Genetic testing (karyotype, Y-chromosome microdeletion, CFTR mutation analysis for obstructive azoospermia) is indicated for severe oligozoospermia or azoospermia. Scrotal ultrasound evaluates for varicocele and testicular pathology. The evaluation produces a diagnostic classification that guides treatment selection.

## Erectile Dysfunction Model

The ED model classifies dysfunction by etiology (vasculogenic, neurogenic, hormonal, psychogenic, medication-induced) and severity (mild, mild-moderate, moderate, severe based on IIEF-5 score). The treatment algorithm follows a stepwise approach: lifestyle modification and risk factor management, PDE5 inhibitor therapy, intracavernosal injection therapy, vacuum erection device, and penile prosthesis implantation. Each step requires documented failure or intolerance before escalation.

## Testosterone Management Model

The testosterone management model tracks the complete TRT lifecycle: confirmatory diagnosis (two morning testosterone measurements below 300 ng/dL with symptoms), contraindication screening (desire for fertility, history of prostate or breast cancer, untreated sleep apnea, erythrocytosis), formulation selection (topical gel, intramuscular injection, subcutaneous pellets), dose titration, and safety monitoring (hematocrit, PSA, lipids, bone density at defined intervals).

## Integration Points

The Male Reproductive Health Context receives physical examination and hormonal data from the Clinical Assessment Context. It sends surgical referrals to the Surgical Management Context for microsurgical procedures. It integrates with endocrinology for complex hormonal management. It coordinates with reproductive medicine for assisted reproduction techniques (IUI, IVF, ICSI) when male factor infertility is identified.

## Business Rules

Infertility evaluation requires at least two abnormal semen analyses before classifying a patient as subfertile. Testosterone replacement therapy requires two confirmed low morning testosterone measurements with associated symptoms before initiation. TRT is contraindicated in men actively trying to conceive without concurrent fertility-preserving therapy (hCG, clomiphene). Hematocrit must be monitored during TRT and therapy modified if it exceeds 54%. Post-vasectomy confirmation requires azoospermia on semen analysis at least 8-16 weeks after the procedure.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Schlegel, P.N. et al. "AUA/ASRM Guideline: Diagnosis and Treatment of Infertility in Men." Journal of Urology, 2021.
- Mulhall, J.P. et al. "AUA Guideline: Evaluation and Management of Testosterone Deficiency." Journal of Urology, 2018.
- World Health Organization. WHO Laboratory Manual for the Examination and Processing of Human Semen. 6th Edition, 2021.
