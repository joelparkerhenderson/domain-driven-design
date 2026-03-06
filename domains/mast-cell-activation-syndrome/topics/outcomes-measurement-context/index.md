# Outcomes Measurement Context for Mast Cell Activation Syndrome

## Overview

The Outcomes Measurement Context is a supporting bounded context that assesses treatment effectiveness and patient well-being over time. This context aggregates data from other bounded contexts to compute symptom burden scores, quality of life metrics, medication effectiveness ratings, and functional status assessments. It serves as the analytical layer of the MCAS domain, providing the evidence base for treatment decisions and management adjustments.

The Outcomes Measurement Context is a downstream consumer that does not own source data. It subscribes to domain events published by the Symptom Tracking, Medication Protocol, Trigger Management, and Diagnostic Assessment contexts, then applies measurement instruments and analytical models to produce outcome assessments.

## Key Aggregates

### Outcomes Assessment

The Outcomes Assessment is the primary aggregate root. It groups all measurement data for a single assessment period into a coherent evaluation. The aggregate contains symptom burden scores, quality of life ratings, medication effectiveness evaluations, and functional status measurements.

The aggregate enforces the invariant that all component scores must be collected before an overall assessment can be finalized. Partial assessments are maintained in a draft state until all required measurements are complete. The assessment also records the evaluation period and the data sources used.

## Value Objects

### Symptom Burden Score

The Symptom Burden Score value object quantifies the cumulative impact of symptoms over a defined period. It contains the overall score, subscores by organ system, the scoring period, and the calculation method used. The score accounts for symptom frequency, intensity, duration, and multi-system involvement.

### Quality of Life Score

The Quality of Life Score value object represents the result of a standardized quality of life assessment. It contains the instrument name (such as SF-36, EQ-5D, or MCAS-specific instruments), the composite score, domain subscores (physical, emotional, social, functional), the assessment date, and the interpretation category (excellent, good, fair, poor).

### Medication Effectiveness Rating

The Medication Effectiveness Rating value object assesses how well a specific medication reduces symptom burden. It contains the medication identifier, the pre-medication symptom burden, the post-medication symptom burden, the change magnitude, and the effectiveness classification (highly effective, moderately effective, minimally effective, ineffective, or worsening).

### Functional Status Rating

The Functional Status Rating value object captures the patient's ability to perform daily activities. It contains ratings for physical function (mobility, strength, endurance), cognitive function (concentration, memory, decision-making), social participation (relationships, community involvement), occupational function (work capacity, productivity), and self-care (activities of daily living).

## Measurement Instruments

### Symptom Burden Assessment

The symptom burden assessment aggregates data from the Symptom Tracking Context. It counts the number of active symptoms, calculates the average severity across organ systems, measures flare frequency and duration, and computes a composite burden score. The assessment uses a standardized scoring algorithm that weights symptoms by their functional impact.

### Quality of Life Measurement

Quality of life measurement uses validated instruments adapted for MCAS. Generic instruments such as the SF-36 or EQ-5D capture overall health-related quality of life. Disease-specific instruments capture MCAS-relevant dimensions such as unpredictability of symptoms, dietary restrictions, activity limitations, and social isolation. The context supports administration and scoring of multiple instruments to provide a comprehensive quality of life picture.

### Medication Effectiveness Analysis

Medication effectiveness is assessed by comparing symptom burden before and after medication changes. The context correlates MedicationStarted, MedicationTitrated, and MedicationDiscontinued events from the Medication Protocol Context with symptom burden changes over defined observation windows. This analysis accounts for confounding factors such as concurrent trigger exposure changes and seasonal symptom variation.

### Functional Status Assessment

Functional status is measured using standardized instruments that assess physical, cognitive, social, and occupational functioning. The context tracks functional status over time to identify improvement, stability, or decline. Functional status assessments are particularly important for MCAS because the condition's impact on daily life often exceeds what traditional symptom severity measures capture.

## Domain Events

OutcomesAssessmentCompleted is published when a comprehensive assessment is finalized. The Specialist Coordination Context uses this event to update the care team on the patient's current status and to inform care plan adjustments. The Medication Protocol Context may use assessment results to justify medication changes.

## Domain Services

The SymptomBurdenCalculationService computes overall and organ-system-specific burden scores from symptom tracking data. It applies validated scoring algorithms and normalizes results for longitudinal comparison.

The TreatmentEffectivenessService evaluates the impact of specific medication changes on patient outcomes. It uses before-and-after comparison methodology with defined observation windows and adjusts for confounding variables.

## Longitudinal Analysis

The Outcomes Measurement Context supports longitudinal trend analysis across multiple assessment periods. Trends in symptom burden, quality of life, and functional status inform long-term treatment planning. The context identifies patterns such as gradual improvement with medication optimization, seasonal variations in outcomes, and the impact of trigger avoidance strategies on overall well-being.

## Population-Level Analysis

Beyond individual patient assessments, the context supports aggregate analysis across patient populations. This enables identification of treatment patterns associated with better outcomes, comparison of medication effectiveness across patient subgroups, and quality improvement initiatives for MCAS management programs.

## Benchmarking

The context maintains reference data for comparing individual patient outcomes against population benchmarks. Benchmarks include typical symptom burden scores for newly diagnosed patients, expected improvement trajectories with standard treatment protocols, and quality of life norms for MCAS patients at various stages of management.

## Reporting

The Outcomes Measurement Context generates standardized reports for clinical use, including treatment effectiveness summaries for care team review, progress reports comparing current and historical assessments, and quality of life trend analyses for shared decision-making with patients.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Jennings, S.V. et al. "The Mastocytosis Society Survey on Mast Cell Disorders: Patient Experiences and Perceptions." Journal of Allergy and Clinical Immunology: In Practice, 2014.
