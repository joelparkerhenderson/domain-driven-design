# Clinical Assessment Context: Attention-Deficit Domain

The Clinical Assessment Context manages the diagnostic evaluation process for ADHD, encompassing referral intake, standardized rating scale administration, neuropsychological testing, clinical interviews, behavioral observations, and diagnostic formulation based on DSM-5 criteria.

## Context Purpose

This bounded context serves as the diagnostic foundation for the entire ADHD management domain. It owns the patient's assessment history and diagnostic profile, providing the clinical evidence base that drives decisions in all downstream contexts: treatment planning, medication selection, educational accommodation, and outcomes measurement. The Clinical Assessment Context answers the fundamental clinical questions: Does this individual meet criteria for ADHD? What is the presentation type? What is the severity? What comorbid conditions are present?

## Aggregate Root: Patient Assessment

The Patient Assessment aggregate encapsulates the complete diagnostic history for an individual. It maintains consistency across all assessment sessions, rating scale administrations, and diagnostic impressions for a single patient. All access to assessment data occurs through this aggregate root.

Key invariants enforced by this aggregate:

- A completed diagnostic evaluation must include assessment data from at least two settings (e.g., home and school).
- Multi-informant data must be collected from at least two informant types (e.g., parent and teacher).
- DSM-5 criteria require evidence that symptoms were present before age 12.
- Diagnostic impression must document the specific DSM-5 criteria met and the evidence supporting each criterion.

## Core Domain Concepts

### Rating Scale Administration

Standardized rating scales are central to ADHD assessment. The context manages the full lifecycle of rating scale usage:

- **Conners Rating Scales (Conners-3, Conners-4)**: Multi-informant scales (parent, teacher, self-report) measuring ADHD symptoms, learning problems, executive functioning, aggression, and peer relations. T-scores above 65 indicate clinically significant concerns.
- **SNAP-IV**: A 26-item scale directly mapping to DSM criteria for inattention and hyperactivity-impulsivity. Used to quantify symptom severity on a 0-3 scale per item.
- **Vanderbilt Assessment Scales**: Parent and teacher forms assessing DSM-based ADHD symptoms and common comorbidities including oppositional defiant disorder, conduct disorder, and anxiety/depression.
- **BRIEF (Behavior Rating Inventory of Executive Function)**: Assesses executive function behaviors in home and school settings across eight clinical scales.

### Neuropsychological Testing

Comprehensive evaluation of cognitive functions relevant to ADHD:

- **Continuous Performance Tests (CPT-3, TOVA)**: Computerized measures of sustained attention, response inhibition, and vigilance over 14-20 minutes. Measures include omission errors (inattention), commission errors (impulsivity), reaction time variability, and detectability.
- **Working Memory Assessment**: Digit span, spatial span, and n-back tasks measuring the ability to hold and manipulate information, a core executive function deficit in ADHD.
- **Processing Speed Evaluation**: Symbol search, coding, and trail-making tasks measuring the speed of cognitive processing, often impaired in the inattentive presentation.

### Diagnostic Formulation

The diagnostic formulation integrates all assessment data into a clinical impression:

- Symptom count verification against DSM-5 threshold (six or more symptoms for children, five or more for adults aged 17 and older).
- Cross-setting impairment documentation showing symptoms cause functional difficulties in two or more settings.
- Developmental history review confirming symptom onset before age 12.
- Differential diagnosis ruling out alternative explanations (anxiety, mood disorders, learning disabilities, medical conditions).
- Comorbidity identification with prevalence-informed screening for commonly co-occurring conditions.

## Domain Events Produced

- **AssessmentCompleted**: Triggers treatment planning and medication evaluation.
- **DiagnosticImpressionRevised**: Triggers review of all active plans and interventions.
- **RatingScaleAdministered**: Provides data to monitoring and outcomes contexts.

## Upstream/Downstream Relationships

- **Upstream to Treatment Management**: Supplies diagnostic data through a Customer-Supplier relationship.
- **Upstream to Medication Management**: Provides diagnostic confirmation and clinical recommendations.
- **Upstream to Educational Accommodation**: Assessment reports are translated through an Anti-Corruption Layer into educational eligibility language.
- **Upstream to Outcomes Tracking**: Baseline assessment data establishes the pre-treatment measurement against which outcomes are compared.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- American Psychiatric Association. (2013). *Diagnostic and Statistical Manual of Mental Disorders* (5th ed.).
- Conners, C. K. (2008). *Conners 3rd Edition Manual*. Multi-Health Systems.
- Swanson, J. M. (1992). *School-Based Assessments and Interventions for ADD Students*. KC Publishing.
