# Domain Services

Domain services in the heart health metrics domain are stateless operations that encapsulate cardiac health business logic spanning multiple aggregates. They represent domain behaviors that do not naturally belong to a single entity or value object but are essential to the cardiac monitoring domain.

## Overview

Eric Evans defines a domain service as an operation that is significant to the domain but does not conceptually belong to any entity or value object. In the heart health metrics domain, domain services perform complex cardiac health computations that require data from multiple aggregates, coordinate workflows that span aggregate boundaries, and implement domain logic that is inherently procedural rather than entity-centric.

Domain services in cardiac health are particularly important because many clinical computations require synthesizing data from disparate sources: a risk score requires demographics from the patient profile, blood pressure trends from monitoring series, lipid values from laboratory results, and smoking status from clinical history. No single aggregate owns all this data.

## Key Concepts

**Rhythm Classification Service.** Analyzes ECG waveform data from an ECG Recording Session aggregate to classify the cardiac rhythm. It applies pattern recognition algorithms to identify normal sinus rhythm, atrial fibrillation, atrial flutter, ventricular tachycardia, and other rhythm types. The service is stateless: it accepts waveform data and returns a Rhythm Classification value object.

**HRV Computation Service.** Computes heart rate variability metrics from a series of R-R Interval value objects. It calculates time-domain measures (SDNN, RMSSD, pNN50), frequency-domain measures (LF power, HF power, LF/HF ratio), and nonlinear measures (Poincare plot indices). The service is stateless and produces an HRV Metrics value object.

**Risk Score Calculation Service.** Orchestrates cardiovascular risk score computation by gathering inputs from multiple aggregates (Patient Cardiac Profile for demographics, Blood Pressure Series for BP values, laboratory results for lipid panels) and applying a validated scoring model (Framingham, ASCVD). The service is stateless and produces a Risk Score value object.

**Alert Evaluation Service.** Evaluates incoming cardiac measurements against all active Alert Subscription aggregates for a patient. It determines whether any threshold has been crossed, computes the severity level, and initiates notification workflows. The service coordinates between the measurement data and the subscription rules without owning either.

**BP Trend Analysis Service.** Analyzes a sequence of Blood Pressure Readings across a Blood Pressure Series to compute trends, detect patterns (white coat hypertension, masked hypertension, nocturnal dipping), and classify the patient's blood pressure status according to ACC/AHA staging criteria.

**Clinical Interpretation Service.** Synthesizes findings from cardiac analysis, risk assessment, and monitoring into a unified clinical interpretation suitable for physician review. It applies clinical decision support rules to prioritize findings, suggest follow-up actions, and flag discordant results.

## Domain Examples

When a 24-hour Holter monitor recording is uploaded, the Rhythm Classification Service processes the entire recording to identify rhythm segments: 18 hours of normal sinus rhythm, 4 hours of atrial fibrillation, and 2 hours of sinus bradycardia during sleep. The service produces a sequence of Rhythm Classification value objects with onset times, durations, and confidence scores.

When a patient completes a home blood pressure monitoring protocol, the BP Trend Analysis Service analyzes the complete series. It identifies that morning readings average 142/88 mmHg (Stage 2 hypertension) while evening readings average 128/78 mmHg (elevated), suggesting a morning surge pattern. The service produces a BP Trend Analysis value object that the Risk Assessment Context uses for risk score recalculation.

## Relationships to Other Topics

- [Aggregates](../aggregates/index.md) - Domain services coordinate across multiple aggregates.
- [Value Objects](../value-objects/index.md) - Domain services accept and return value objects.
- [Repositories](../repositories/index.md) - Domain services use repositories to access aggregates.
- [Domain Events](../domain-events/index.md) - Domain services may trigger domain event publication.
- [Cardiac Analysis Context](../cardiac-analysis-context/index.md) - Home to rhythm and HRV services.
- [Risk Assessment Context](../risk-assessment-context/index.md) - Home to risk score calculation services.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 9 on domain services.
- D'Agostino, R.B. et al. "General Cardiovascular Risk Profile for Use in Primary Care: The Framingham Heart Study." *Circulation*, 117(6), 743-753, 2008.
- Task Force of the European Society of Cardiology. "Heart Rate Variability: Standards of Measurement." *Circulation*, 93(5), 1043-1065, 1996.
