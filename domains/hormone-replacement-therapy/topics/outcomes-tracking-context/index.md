# Outcomes Tracking Context in the Hormone Replacement Therapy Domain

The Outcomes Tracking Context is a bounded context responsible for measuring and evaluating the results of hormone replacement therapy (HRT) across multiple dimensions. It tracks symptom resolution rates, quality of life scores, long-term health outcomes, and overall treatment efficacy, aggregating longitudinal data to assess whether treatment is achieving its intended goals.

## Context Overview

Outcomes tracking closes the feedback loop in HRT management. While the Hormone Protocol Context defines what treatment is given and the Lab Monitoring Context measures biochemical response, the Outcomes Tracking Context evaluates whether the treatment is actually making the patient better. It captures patient-reported outcomes, clinician-assessed improvements, and objective health measurements over time, providing the evidence base that informs treatment continuation, modification, and clinical learning.

## Domain Model

### Aggregates

The TreatmentOutcome aggregate is the primary aggregate root, tracking longitudinal treatment results for a patient's treatment episode. It accumulates OutcomeMeasurement entities over time and enforces the invariant that measurements must be in chronological order with each referencing a valid assessment instrument.

### Key Entities

The TreatmentOutcome entity represents the complete outcome record for a treatment episode, growing throughout the treatment lifecycle and potentially persisting after treatment completion for long-term follow-up. The OutcomeMeasurement entity captures a single measurement at a specific time point, recording the instrument, scores, and calculated results.

### Key Value Objects

QualityOfLifeScore represents standardized well-being measurements across physical, emotional, and functional domains. SymptomResolutionRate quantifies the degree of improvement from baseline for specific symptom categories. EfficacyRating provides an overall assessment of treatment effectiveness.

## Outcome Dimensions

### Symptom Resolution

The primary outcome for most HRT patients is relief from presenting symptoms. Symptom resolution is measured by re-administering the same validated instruments used during baseline clinical assessment at defined follow-up intervals. For menopausal symptoms, the Menopause Rating Scale or Greene Climacteric Scale tracks vasomotor symptoms (hot flashes, night sweats), psychological symptoms (mood, sleep, cognitive function), and urogenital symptoms. The SymptomResolutionRate value object calculates the percentage improvement from baseline.

### Quality of Life

Quality of life measurement captures the broader impact of treatment on patient well-being using validated instruments such as the SF-36 (Short Form Health Survey), the MENQOL (Menopause-Specific Quality of Life), or the EQ-5D (EuroQol five-dimension). These instruments assess physical functioning, role limitations, bodily pain, general health, vitality, social functioning, emotional well-being, and mental health. The QualityOfLifeScore value object captures domain scores and composite scores.

### Metabolic Health Outcomes

Long-term metabolic outcomes track the impact of HRT on metabolic parameters over time. Relevant measurements include fasting glucose and insulin sensitivity trends, lipid profile changes (total cholesterol, LDL, HDL, triglycerides), body composition changes (weight, body mass index, waist-to-hip ratio), and inflammatory marker trends (high-sensitivity CRP).

### Bone Health Outcomes

For patients at osteoporosis risk, bone health outcomes track the effect of HRT on bone mineral density and fracture risk. DEXA scan results are recorded at baseline and typically every 1-2 years. The context tracks T-scores, Z-scores, and changes from baseline to assess whether HRT is providing expected osteoprotective effects.

### Cardiovascular Health Outcomes

Cardiovascular outcomes monitor the long-term cardiovascular impact of HRT, which is particularly relevant given the findings of the Women's Health Initiative. Tracked parameters include blood pressure trends, lipid fraction changes, carotid intima-media thickness (where assessed), and coronary artery calcium scores (where assessed). The context evaluates whether the patient's cardiovascular risk profile is stable, improving, or worsening during treatment.

## Outcome Assessment Schedule

Outcomes are assessed at standardized intervals aligned with clinical practice. Short-term assessment occurs at 3 months (initial response evaluation). Medium-term assessment occurs at 6 and 12 months (treatment stabilization evaluation). Long-term assessment occurs annually thereafter (ongoing efficacy and safety evaluation). The TreatmentMilestoneReached event is published at each major assessment interval.

## Efficacy Evaluation

The EfficacyCalculationService computes treatment efficacy by comparing current outcomes against baseline values, treatment goals, and evidence-based expected outcomes. Efficacy is rated as highly effective (symptom resolution exceeds 75% with quality of life improvement and stable safety markers), effective (symptom resolution between 50-75% with positive quality of life trends), partially effective (symptom resolution between 25-50% or mixed results across dimensions), or ineffective (less than 25% symptom resolution or deterioration in any safety dimension).

## Population-Level Outcomes

Beyond individual patient outcomes, this context supports aggregate analysis across patient populations. The OutcomeBenchmarkingService compares individual outcomes against population benchmarks, identifying patients who are performing significantly above or below expectations. Population analysis informs treatment protocol refinement, identifies patient subgroups that respond differentially to specific regimens, and supports evidence-based practice improvement.

## Domain Events

The context publishes OutcomeAssessed when a periodic outcome assessment is completed and TreatmentMilestoneReached when a patient reaches a significant treatment time point. These events are consumed primarily by the Treatment Optimization Context to inform evidence-based protocol adjustments.

## Context Relationships

The Outcomes Tracking Context receives data from all other contexts, making it the most downstream context in the HRT domain. It consumes protocol status from Hormone Protocol, lab results from Lab Monitoring, adverse events from Side Effect Management, and protocol modifications from Treatment Optimization. It publishes outcome data to Treatment Optimization through published language for evidence-based adjustment recommendations.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Ware, J.E., and Sherbourne, C.D. "The MOS 36-Item Short-Form Health Survey (SF-36)." Medical Care, 1992.
- Hilditch, J.R., et al. "A Menopause-Specific Quality of Life Questionnaire." Maturitas, 1996.
