# Oncologic Urology Context

## Overview

The Oncologic Urology Context manages the full lifecycle of urological malignancies from initial detection through long-term surveillance. This bounded context owns the clinical models for prostate cancer, bladder cancer, kidney cancer, and testicular cancer, including staging classifications, risk stratification algorithms, treatment selection protocols, and surveillance schedules. It integrates with pathology, radiology, medical oncology, and radiation oncology to deliver coordinated cancer care.

## Scope

This context covers cancer screening (PSA testing, risk calculators), diagnostic biopsy protocols (systematic prostate biopsy, MRI-fusion targeted biopsy, bladder biopsy, renal mass biopsy), pathological grading (Gleason score, ISUP Grade Groups, Fuhrman/WHO-ISUP nuclear grade), staging (AJCC TNM classification), risk stratification (D'Amico, CAPRA, NCCN risk groups, Leibovich score), treatment planning (active surveillance, surgery, radiation, systemic therapy), and post-treatment surveillance.

## Ubiquitous Language

"Active surveillance" is a defined management strategy with specific entry criteria, monitoring schedules, and intervention triggers, distinct from "watchful waiting." "Biochemical recurrence" indicates a rising PSA after definitive treatment, defined as PSA greater than 0.2 ng/mL after prostatectomy or nadir plus 2.0 ng/mL after radiation. "Risk group" classifies cancer aggressiveness using validated instruments. "Surveillance protocol" is a structured schedule of monitoring tests with defined intervals and reclassification criteria.

## Aggregates

The CancerDiagnosis aggregate root manages a patient's cancer from detection through staging. It contains BiopsyResult entities, StagingClassification value objects, GleasonScore value objects, and RiskStratification value objects. The SurveillancePlan aggregate root manages long-term monitoring with ScheduledVisit entities and SurveillanceResult entities. The TreatmentPlan aggregate manages the selected treatment approach with its expected milestones and outcomes.

## Prostate Cancer Model

The prostate cancer model is the most complex within this context. It tracks PSA history as a time series, biopsy results with core-level detail, MRI findings with PI-RADS scoring, genomic test results (Decipher, Oncotype DX Prostate, Prolaris), and multiparametric risk assessment. The model supports the full spectrum from active surveillance for low-risk disease through multimodal therapy for locally advanced and metastatic disease.

## Bladder Cancer Model

The bladder cancer model distinguishes non-muscle-invasive bladder cancer (NMIBC) from muscle-invasive bladder cancer (MIBC). For NMIBC, it tracks TURBT findings, intravesical therapy protocols (BCG induction, BCG maintenance), and cystoscopic surveillance with defined intervals based on AUA/EAU risk tables. For MIBC, it manages neoadjuvant chemotherapy, radical cystectomy with urinary diversion selection, and adjuvant therapy decisions.

## Kidney Cancer Model

The kidney cancer model manages renal mass characterization (Bosniak classification for cystic lesions, enhancement patterns for solid lesions), biopsy results when obtained, surgical planning (partial versus radical nephrectomy, ablation), and surveillance for small renal masses under active surveillance. The model incorporates nephrometry scoring (R.E.N.A.L., PADUA) to quantify surgical complexity.

## Testicular Cancer Model

The testicular cancer model tracks serum tumor markers (AFP, beta-HCG, LDH), staging, risk classification (IGCCCG), orchiectomy pathology, and surveillance or adjuvant therapy protocols. The model supports the rapid clinical decision-making required for this aggressive but highly curable malignancy.

## Domain Events

BiopsyResultReceived triggers staging updates and treatment planning. CancerStageUpdated propagates stage changes to the surveillance and treatment aggregates. PSAThresholdBreached triggers clinical review for potential biochemical recurrence. SurveillanceReclassificationTriggered indicates that surveillance findings warrant intervention consideration. TreatmentCompleted initiates the transition from active treatment to post-treatment surveillance.

## Integration Points

The Oncologic Context integrates with pathology systems for biopsy and surgical specimen results through an anti-corruption layer that translates generic pathology reports into structured oncologic grading. It integrates with the Clinical Assessment Context for diagnostic imaging. It sends surgical requests to the Surgical Management Context and receives operative outcomes. It coordinates with medical oncology for systemic therapy and with radiation oncology for radiotherapy planning.

## Business Rules

Cancer staging must follow the current AJCC TNM classification edition. Risk stratification must use validated instruments with documented version numbers. Active surveillance eligibility criteria must be explicitly defined and consistently applied. Surveillance intervals must follow protocol unless a documented clinical justification for deviation exists. Multidisciplinary tumor board review is required for all cases where the recommended treatment deviates from guideline-concordant care.

## Relationship to Other Contexts

The Oncologic Urology Context is a customer of the Clinical Assessment Context for diagnostic data and a customer of the Surgical Management Context for procedural execution. It is an upstream context to pathology and radiology systems through anti-corruption layers. It maintains a conformist relationship with AJCC/UICC staging standards, adopting their classification system without modification.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- NCCN Clinical Practice Guidelines in Oncology: Prostate Cancer, Bladder Cancer, Kidney Cancer. https://www.nccn.org
- Amin, M.B. et al. AJCC Cancer Staging Manual. 8th Edition. Springer, 2017.
- Epstein, J.I. et al. "The 2014 ISUP Gleason Grading Conference." American Journal of Surgical Pathology, 2016.
