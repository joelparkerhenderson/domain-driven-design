# Respiratory Diagnostics Context

## Overview

The Respiratory Diagnostics Context governs the ordering, execution, and interpretation of diagnostic tests for respiratory conditions. It handles pulmonary function tests (spirometry, body plethysmography, diffusion capacity), chest imaging interpretation, bronchoscopy procedures, and sputum analysis. This bounded context is classified as a core subdomain because diagnostic interpretation requires specialized clinical algorithms that encode deep pulmonary medicine expertise.

This context receives assessment data from the Clinical Assessment Context to inform test selection and publishes diagnostic results that drive care plan decisions in the Chronic Disease Management Context.

## Ubiquitous Language

Key terms within this context include: diagnostic order, spirometry, FEV1, FVC, FEV1/FVC ratio, percent predicted, body plethysmography, total lung capacity, residual volume, DLCO, diffusion capacity, bronchoscopy, bronchoalveolar lavage, transbronchial biopsy, sputum culture, sputum cytology, chest radiograph, CT thorax, HRCT, diagnostic report, test quality grade, reversibility test, bronchodilator response, and methacholine challenge.

## Aggregate: Diagnostic Order

The Diagnostic Order is the primary aggregate root, representing a request for one or more diagnostic tests.

**Components**:
- Order ID (unique identifier)
- Patient ID (reference)
- Ordering clinician ID
- Order date and time
- Clinical indication (reason for testing)
- Ordered tests (list of test types with specific requirements)
- Patient preparation instructions (e.g., bronchodilator withholding for reversibility testing)
- Order status (pending, scheduled, in-progress, completed, cancelled)
- Priority (routine, urgent)

**Invariants**:
- A reversibility test order must include documentation that short-acting bronchodilators are to be withheld for the specified period.
- Each ordered test must have a valid clinical indication.
- An order cannot transition to completed status until all ordered tests have results.

## Aggregate: Diagnostic Report

The Diagnostic Report is the second aggregate root, representing the collected results and interpretations of completed tests.

**Components**:
- Report ID (unique identifier)
- Patient ID (reference)
- Associated order ID (reference)
- Test results (spirometry results, imaging findings, bronchoscopy findings, microbiology results)
- Quality assessment for each test
- Interpreting clinician ID
- Interpretation narrative
- Report status (preliminary, final, amended)
- Report date and time

**Invariants**:
- A finalized report must include quality assessment for all included tests.
- Spirometry results must include both pre-bronchodilator and post-bronchodilator values when a reversibility test was ordered.
- An amended report must reference the original report and document the reason for amendment.

## Entities

**Test Execution Record**: Tracks the execution of a specific test, including the technician performing the test, equipment identifier, calibration status, raw measurements, and quality indicators (acceptability and repeatability criteria for spirometry per ATS/ERS standards).

**Bronchoscopy Report**: Documents bronchoscopy procedure findings including airway inspection results, biopsy sites and findings, lavage specimen details, and complications. Maintained as a distinct entity due to its procedural complexity.

## Value Objects

**SpirometryResult**: Captures FEV1 (liters and percent predicted), FVC (liters and percent predicted), FEV1/FVC ratio, PEF, and flow-volume loop classification. Includes pre- and post-bronchodilator values when applicable. Self-validates measurement consistency.

**ArterialBloodGas**: Captures pH, PaO2, PaCO2, HCO3, base excess, and SaO2 with acid-base interpretation.

**DLCOResult**: Captures diffusion capacity measured value, percent predicted, and interpretation (normal, mildly reduced, moderately reduced, severely reduced).

**ImagingFinding**: Captures modality (chest X-ray, CT, HRCT), anatomical location, finding description, and clinical significance.

**SputumAnalysis**: Captures macroscopic appearance, culture results, sensitivity patterns, cytology findings, and AFB stain results.

**TestQualityGrade**: Represents the quality assessment of a pulmonary function test per ATS/ERS acceptability and repeatability criteria (grades A through F).

## Domain Events

**DiagnosticOrderCreated**: Published when a new diagnostic order is placed. Consumed by scheduling systems and the test execution workflow.

**DiagnosticResultAvailable**: Published when a diagnostic report is finalized. Carries key results and interpretation. Consumed by the Chronic Disease Management Context and Clinical Assessment Context.

**AbnormalResultFlagged**: Published when results indicate clinically significant abnormalities requiring attention.

## Domain Services

**SpirometryInterpretationService**: Applies ATS/ERS interpretation algorithms to classify spirometric patterns as normal, obstructive, restrictive, or mixed, and grades severity using percent predicted values and lower limits of normal.

**DiagnosticCorrelationService**: Integrates results from multiple test modalities to identify concordant or discordant findings and produce a comprehensive diagnostic picture.

## Integration Points

- **Upstream**: Receives assessment data from Clinical Assessment Context via domain events. Receives laboratory results from LIS through an anti-corruption layer.
- **Downstream**: Publishes diagnostic results consumed by Chronic Disease Management and Clinical Assessment contexts.
- **External**: Interfaces with laboratory systems, radiology PACS, and pulmonary function test equipment through anti-corruption layers.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Graham, B. L., et al. (2019). *Standardization of Spirometry 2019 Update*. American Journal of Respiratory and Critical Care Medicine.
- Stanojevic, S., et al. (2022). *ERS/ATS Technical Standard on Interpretive Strategies for Routine Lung Function Tests*. European Respiratory Journal.
