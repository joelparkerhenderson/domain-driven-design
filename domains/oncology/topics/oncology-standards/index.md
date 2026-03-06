# Oncology Standards

## Overview

Clinical standards are foundational to the oncology domain model. They serve as published languages in DDD terms, providing shared vocabularies, classification systems, and evidence-based guidelines that multiple bounded contexts reference. Standards reduce translation overhead between contexts because they carry consistent meaning across organizational boundaries. In the oncology domain, standards inform value object design, constrain aggregate behavior, and anchor the ubiquitous language in clinically validated terminology.

## AJCC TNM Staging System

The American Joint Committee on Cancer (AJCC) TNM staging system is the international standard for classifying cancer extent. It assigns categories for Tumor size and extent (T), regional lymph Node involvement (N), and distant Metastasis (M). These categories are combined into an overall stage group (Stage 0, I, II, III, IV with subgroups).

In the domain model, the TNM Classification is modeled as a value object within the Diagnosis and Staging Context. It contains the T category, N category, M category, the AJCC edition used (currently 8th edition), and the staging basis (clinical staging from imaging versus pathological staging from surgical specimens). The StagingService domain service applies AJCC lookup rules to compute the overall stage group from individual categories.

The TNM system is cancer-type specific. The T categories for breast cancer have different definitions than the T categories for lung cancer. The domain model must capture this specificity, ensuring that TNM values are validated against the rules for the specific cancer type being staged.

TNM staging impacts every bounded context. The Treatment Planning Context uses the stage to determine guideline-recommended treatment options. The Chemotherapy Management Context may reference the stage for protocol eligibility. The Survivorship Care Context uses the stage to determine appropriate surveillance intensity.

## NCCN Clinical Practice Guidelines

The National Comprehensive Cancer Network (NCCN) guidelines provide evidence-based, consensus-driven recommendations for cancer treatment and management. For each cancer type, NCCN guidelines define decision pathways that map clinical characteristics (stage, molecular markers, performance status) to recommended treatment options.

In the domain model, NCCN guideline references are modeled as value objects that identify the specific guideline, version, cancer type, and pathway. The GuidelineRecommendationService domain service maps a patient's clinical profile to the applicable NCCN pathway recommendations.

NCCN guidelines are updated multiple times per year. The domain model must track which guideline version was referenced when a treatment plan was created. This versioning is clinically and legally significant: a treatment plan should be evaluated against the guidelines in effect at the time of planning, not retrospectively against subsequently updated guidelines.

NCCN guidelines serve as a published language between the Treatment Planning Context and the treatment modality contexts. When a treatment plan references "NCCN Category 1 recommendation," all contexts share an understanding that this represents a recommendation based on high-level evidence with uniform consensus.

## CTCAE (Common Terminology Criteria for Adverse Events)

The CTCAE is the National Cancer Institute's standardized classification for adverse events related to cancer treatment. It defines a grading scale from Grade 1 (mild) through Grade 5 (death), with specific criteria for each adverse event term and grade combination.

In the domain model, the CTCAE Grade is modeled as a value object within the Chemotherapy Management Context, containing the CTCAE version, the adverse event term, the system organ class, and the grade. The DoseModificationService uses CTCAE grades as input to determine appropriate dose adjustments.

CTCAE serves as a published language across contexts. The Chemotherapy Management Context records toxicities using CTCAE terms. The Treatment Planning Context understands these terms when evaluating whether treatment modification is needed. The Survivorship Care Context uses CTCAE terms to document treatment-related complications in the care plan.

## ECOG and Karnofsky Performance Status

Performance status scales measure a patient's functional capacity and influence treatment eligibility and intensity. The ECOG (Eastern Cooperative Oncology Group) scale ranges from 0 (fully active) to 4 (completely disabled). The Karnofsky scale ranges from 100 (normal) to 0 (dead).

In the domain model, Performance Status is a value object used across multiple contexts. The Treatment Planning Context considers performance status when evaluating treatment options. The Chemotherapy Management Context may use it to determine dose intensity. The Survivorship Care Context tracks performance status over time as a quality of life indicator.

## WHO Classification of Tumours

The World Health Organization Classification of Tumours provides the authoritative histological classification system for all tumor types. It defines the diagnostic criteria, grading systems, and molecular subtypes for each tumor category.

In the domain model, the histological classification from the WHO system informs the cancer type identification within the Cancer Diagnosis aggregate. The specific WHO tumor type, combined with the grade, determines which AJCC staging rules apply and which NCCN treatment guidelines are relevant.

## RECIST (Response Evaluation Criteria in Solid Tumors)

RECIST provides standardized criteria for evaluating tumor response to treatment based on changes in tumor measurements on imaging. Response categories include complete response, partial response, stable disease, and progressive disease.

In the domain model, RECIST response assessments are value objects used primarily in the Treatment Planning Context and the Chemotherapy Management Context to evaluate treatment effectiveness and determine whether to continue, modify, or change the treatment approach.

## ICD-O (International Classification of Diseases for Oncology)

ICD-O provides standardized coding for tumor topography (anatomic site) and morphology (histological type and behavior). It is primarily used for cancer registration and epidemiological reporting.

In the domain model, ICD-O codes are generated by the anti-corruption layer between the Diagnosis and Staging Context and tumor registry systems. The oncology domain model uses richer representations internally (WHO classification, AJCC staging) and translates to ICD-O codes only for registry reporting.

## Standards Governance

Clinical standards evolve as medical knowledge advances. The domain model must accommodate standard version changes through explicit versioning of all standard references. When a new AJCC staging edition is published, existing diagnoses retain their original staging version while new diagnoses use the updated rules. When NCCN guidelines are revised, existing treatment plans reference the version used at planning time while new plans reference the current version.

## References

- American Joint Committee on Cancer. AJCC Cancer Staging Manual, 8th Edition. Springer, 2017.
- National Comprehensive Cancer Network. NCCN Clinical Practice Guidelines. https://www.nccn.org/guidelines
- National Cancer Institute. Common Terminology Criteria for Adverse Events (CTCAE) v5.0. 2017.
- World Health Organization. WHO Classification of Tumours, 5th Edition. IARC Press, 2019-2024.
- Eisenhauer, E.A., et al. New Response Evaluation Criteria in Solid Tumours: RECIST Guideline (version 1.1). European Journal of Cancer, 2009.
- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
