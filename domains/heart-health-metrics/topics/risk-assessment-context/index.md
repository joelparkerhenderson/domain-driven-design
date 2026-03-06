# Risk Assessment Context

The Risk Assessment Context is the bounded context responsible for evaluating cardiovascular risk using validated scoring models, incorporating comorbidity factors, and producing risk stratification results that inform clinical decision-making. It synthesizes cardiac analysis findings, patient demographics, laboratory results, and lifestyle factors into actionable risk assessments.

## Overview

The Risk Assessment Context occupies a central position in the heart health metrics domain, translating raw cardiac data and analysis findings into clinically validated risk predictions. It implements established cardiovascular risk scoring algorithms such as the Framingham Risk Score and the ASCVD (Atherosclerotic Cardiovascular Disease) Risk Calculator, as well as institution-specific or population-adapted scoring models.

This context is classified as a core subdomain because accurate risk prediction directly impacts clinical outcomes: it determines which patients receive preventive interventions, medication adjustments, or referrals for advanced cardiac evaluation. The sophistication and validation rigor of the risk models are a primary differentiator for cardiac health monitoring platforms.

## Key Concepts

**Framingham Risk Score.** A sex-specific algorithm estimating 10-year cardiovascular event risk based on age, total cholesterol, HDL cholesterol, systolic blood pressure, blood pressure treatment status, smoking status, and diabetes status. The context implements the validated Framingham equations and applies them to patient data, producing a Risk Score value object with the computed percentage and risk category.

**ASCVD Risk Calculator.** The Pooled Cohort Equations recommended by ACC/AHA for estimating 10-year and lifetime atherosclerotic cardiovascular disease risk. It considers age, sex, race, total cholesterol, HDL cholesterol, systolic blood pressure, blood pressure treatment, diabetes, and smoking. The context supports both the original equations and subsequent validation cohort adjustments.

**Comorbidity Weighting.** The context incorporates comorbidity factors that modify baseline cardiovascular risk: diabetes mellitus, chronic kidney disease, family history of premature cardiovascular disease, metabolic syndrome, inflammatory conditions, and obesity. Each comorbidity has a defined impact on the risk calculation.

**Risk Stratification.** Computed risk scores are classified into risk tiers: low risk, borderline risk, intermediate risk, and high risk. Stratification thresholds follow published clinical guidelines and may be configurable for institutional preferences. Risk category determines recommended follow-up intervals, intervention aggressiveness, and monitoring intensity.

**Risk Profile Aggregate.** The central aggregate in this context, encapsulating a patient's complete risk evaluation: input parameters, scoring model applied, computed scores, risk category, contributing factors, and recommendations. The aggregate ensures all required inputs are present and valid before computing a score.

**Longitudinal Risk Tracking.** The context supports tracking risk score changes over time, enabling clinicians to assess whether interventions (medication, lifestyle changes) are reducing cardiovascular risk. Each risk assessment is stored with its input snapshot, allowing for retrospective analysis and trend detection.

## Domain Examples

A 55-year-old male patient with total cholesterol 240 mg/dL, HDL 42 mg/dL, systolic BP 148 mmHg (on treatment), non-smoker, non-diabetic is evaluated using the ASCVD Risk Calculator. The Risk Assessment Context gathers demographic data from the Patient Cardiac Profile, BP data from the Cardiac Analysis Context, and laboratory values from clinical integration. It computes a 10-year ASCVD risk of 14.2%, classifying the patient as intermediate risk. The Risk Score Updated domain event is published.

Six months later, after medication adjustments and lifestyle changes, the patient's total cholesterol is 198 mg/dL and systolic BP is 132 mmHg. The Risk Assessment Context recalculates the ASCVD risk at 9.8%, showing a reduction to borderline risk. The longitudinal tracking shows a favorable trend.

## Relationships to Other Topics

- [Cardiac Analysis Context](../cardiac-analysis-context/index.md) - Provides BP trends and cardiac metrics for risk input.
- [Domain Services](../domain-services/index.md) - Risk Score Calculation Service implements scoring logic.
- [Aggregates](../aggregates/index.md) - Risk Profile is a key aggregate in this context.
- [Value Objects](../value-objects/index.md) - Risk Score is a central value object.
- [Domain Events](../domain-events/index.md) - Risk Score Updated is a key domain event.
- [Subdomains](../subdomains/index.md) - Classified as a core subdomain.
- [Reporting & Visualization Context](../reporting-visualization-context/index.md) - Downstream consumer of risk results.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021.
- D'Agostino, R.B. et al. "General Cardiovascular Risk Profile for Use in Primary Care: The Framingham Heart Study." *Circulation*, 117(6), 743-753, 2008.
- Goff, D.C. et al. "2013 ACC/AHA Guideline on the Assessment of Atherosclerotic Cardiovascular Risk." *Journal of the American College of Cardiology*, 63(25), 2935-2959, 2014.
- Arnett, D.K. et al. "2019 ACC/AHA Guideline on the Primary Prevention of Cardiovascular Disease." *Circulation*, 140(11), e596-e646, 2019.
