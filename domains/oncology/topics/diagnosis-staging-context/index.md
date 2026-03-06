# Diagnosis and Staging Context

## Overview

The Diagnosis and Staging Context encompasses all activities related to identifying cancer, characterizing its biological behavior, and classifying its extent. This bounded context is the entry point of the oncology care continuum, establishing the foundational clinical picture that drives all downstream treatment decisions. It includes cancer screening programs, diagnostic imaging interpretation, biopsy procedures, pathology analysis, TNM staging classification, and molecular profiling.

## Ubiquitous Language

Within this context, specific terms carry precise meanings. A "diagnosis" is the confirmed identification of a malignant neoplasm based on pathological examination. "Staging" is the systematic classification of cancer extent using the AJCC TNM system. A "molecular profile" is the set of genetic and protein markers identified in the tumor that inform treatment selection. A "pathology report" is the authoritative document produced by a pathologist that establishes the histological diagnosis, grade, and biomarker status.

## Aggregate Roots

### Cancer Diagnosis

The Cancer Diagnosis is the primary aggregate root. It encapsulates the confirmed cancer type (histology and primary site), tumor grade, TNM classification, overall stage group, molecular profile results, and the pathology report reference. The aggregate enforces the invariant that the TNM classification and overall stage group are consistent with AJCC rules for the specific cancer type.

When molecular profiling results arrive after initial staging, the Cancer Diagnosis aggregate incorporates them without altering the anatomic stage. If new imaging or surgical findings change the T, N, or M categories, the aggregate recomputes the overall stage group and publishes a StageRevised domain event.

### Pathology Case

The Pathology Case aggregate manages the lifecycle of a diagnostic evaluation from specimen collection through final reporting. It contains the specimens collected, tissue blocks prepared, stains performed, and the structured pathology report. The aggregate enforces that the report cannot be finalized until all required synoptic elements are completed (per College of American Pathologists protocols).

## Key Entities

**Tumor**: Represents a specific neoplasm identified in the patient. A patient may have multiple tumors, each independently staged and tracked. The tumor entity carries the anatomic site, laterality, and histological classification.

**Biopsy Specimen**: Represents a tissue sample obtained for diagnostic evaluation. Tracked from collection through processing, examination, and reporting. Each specimen has a unique accession number.

**Screening Episode**: Represents a cancer screening encounter, including the screening modality (mammography, colonoscopy, low-dose CT, PSA), the result, and any findings requiring diagnostic follow-up.

## Key Value Objects

**TNM Classification**: The staging classification composed of T category, N category, M category, and the AJCC edition used. Validates that the category combination is valid for the cancer type.

**Histological Grade**: The degree of cellular differentiation, expressed using the appropriate grading system for the tumor type.

**Biomarker Result**: The outcome of a specific biomarker test, including the test methodology, measured value, and clinical interpretation.

**Molecular Mutation**: A specific genetic alteration identified in the tumor, with its variant classification.

## Domain Events

**ScreeningResultReceived**: A screening test result has been recorded.
**BiopsySpecimenCollected**: A tissue sample has been obtained.
**PathologyReportFinalized**: The pathologist has completed the diagnostic report.
**DiagnosisConfirmed**: The cancer diagnosis is formally confirmed with complete staging.
**StageRevised**: Staging has been updated based on new clinical or pathological information.
**MolecularProfileCompleted**: Molecular profiling results are available.

## Domain Services

**StagingService**: Computes the overall stage group from TNM category values using AJCC rules for the specific cancer type.

**MolecularProfileInterpretationService**: Evaluates molecular test results to identify clinically actionable findings for the specific cancer type.

## Integration Points

This context is upstream to the Treatment Planning Context, publishing diagnosis and staging events that trigger treatment planning workflows. It receives laboratory results through an anti-corruption layer that translates generic lab data into typed oncology value objects. It receives imaging study results through an ACL that extracts staging-relevant findings. It publishes to tumor registries through an ACL that translates rich diagnostic data into coded registry formats (ICD-O topography and morphology codes).

## Clinical Standards

The AJCC Cancer Staging Manual (8th Edition) governs TNM classification rules. The WHO Classification of Tumours provides the authoritative histological classification system. The College of American Pathologists Cancer Protocols define synoptic reporting requirements. Biomarker testing follows ASCO/CAP guidelines for specific markers (HER2, PD-L1, MSI).

## Consistency Boundaries

Within this context, the Cancer Diagnosis aggregate maintains strict consistency: the TNM classification and overall stage are always synchronized. Across context boundaries, eventual consistency applies. When a diagnosis is confirmed, the Treatment Planning Context is notified asynchronously through domain events and may initiate treatment planning on its own timeline.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Joint Committee on Cancer. AJCC Cancer Staging Manual, 8th Edition. Springer, 2017.
- College of American Pathologists. Cancer Protocols and Checklists. https://www.cap.org/protocols-and-guidelines
