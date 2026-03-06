# Ubiquitous Language

The ubiquitous language for the heart health metrics domain establishes a shared vocabulary used consistently by cardiologists, nurses, biomedical engineers, data scientists, and software developers. This language bridges the gap between clinical cardiology expertise and software design, ensuring that domain models faithfully represent cardiac health concepts.

## Overview

Eric Evans introduced the concept of ubiquitous language as a language structured around the domain model and used by all team members to connect their activities with the software. In the heart health metrics domain, the ubiquitous language draws from established cardiology terminology, clinical measurement standards, and health informatics vocabularies. Every term has a precise definition agreed upon by both clinical and technical stakeholders.

Maintaining a rigorous ubiquitous language is especially important in cardiac health because ambiguity can have clinical consequences. A "normal" heart rate means different things for an athlete versus a sedentary patient. "Elevated blood pressure" has specific clinical thresholds defined by ACC/AHA guidelines. The ubiquitous language captures these nuances and embeds them in the domain model.

## Key Concepts

**Clinical Measurement Terms.** Heart rate, blood pressure (systolic/diastolic), heart rate variability, and ECG waveform components (P wave, QRS complex, T wave, R-R interval, QT interval) form the foundation of measurement language. Each term carries units, normal ranges, and clinical significance.

**Cardiac Condition Terms.** Arrhythmia, atrial fibrillation, bradycardia, tachycardia, and hypertension describe clinically significant states that the system must detect, classify, and communicate. These terms have formal clinical definitions from organizations like the AHA and ACC.

**Risk Assessment Terms.** Framingham Risk Score, ASCVD Risk Score, cardiovascular risk factor, and risk stratification describe the vocabulary of predictive cardiac health evaluation. These terms are defined by published clinical guidelines with specific calculation methodologies.

**Biomarker Terms.** Troponin, BNP, ejection fraction, and cardiac output represent measurable indicators of cardiac function and damage. Each biomarker has clinical reference ranges and diagnostic thresholds.

**Technology and Standards Terms.** Photoplethysmography, FHIR, LOINC, SNOMED CT, and clinical decision support describe the technical and interoperability vocabulary used in system design and integration.

**Consistency Across Contexts.** While each bounded context may refine certain terms for its internal use, the ubiquitous language ensures that when "atrial fibrillation" is referenced in the Data Collection Context (as a detection target), the Cardiac Analysis Context (as a classification result), or the Monitoring and Alerts Context (as an alert trigger), the term carries the same fundamental clinical meaning.

## Domain Examples

When a biomedical engineer says "the PPG sensor reports a heart rate of 72 bpm with HRV SDNN of 45 ms," every team member understands the measurement source, units, and clinical significance. The software model represents this as a Heart Rate Reading value object with bpm units and an HRV Analysis Result containing SDNN, RMSSD, and frequency-domain components.

When a cardiologist says "the patient's ASCVD 10-year risk is 12%, placing them in the borderline risk category," the risk assessment model captures this using a Risk Score value object with the scoring model identifier, the computed percentage, and the risk stratification tier.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/index.md) - Ubiquitous language is foundational to strategic design decisions.
- [Bounded Contexts](../bounded-contexts/index.md) - Each bounded context refines the ubiquitous language for its scope.
- [Value Objects](../value-objects/index.md) - Many ubiquitous language terms become value objects in the model.
- [Entities](../entities/index.md) - Key domain nouns become entities with unique identity.
- [Heart Health Metrics Standards](../heart-health-metrics-standards/index.md) - Standards define authoritative term definitions.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 2 on ubiquitous language.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 1 on developing the ubiquitous language.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 2 on knowledge discovery.
- Dorland's Illustrated Medical Dictionary. 33rd ed. Elsevier, 2020.
- LOINC Committee. "Logical Observation Identifiers Names and Codes." Regenstrief Institute. https://loinc.org/
- SNOMED International. "SNOMED CT." https://www.snomed.org/
