# Treatment Optimization Context in the Hormone Replacement Therapy Domain

The Treatment Optimization Context is a bounded context responsible for the analytical work of adjusting hormone replacement therapy (HRT) protocols based on accumulated clinical evidence. It performs dose titration calculations, formulation adjustment evaluations, combination therapy planning, and protocol modification recommendations by synthesizing data from laboratory monitoring, side effect management, and outcomes tracking.

## Context Overview

Effective HRT management is an iterative process. Initial protocols are prescribed based on clinical assessment and evidence-based guidelines, but optimal treatment requires ongoing adjustment as patient response data accumulates. The Treatment Optimization Context encodes the clinical reasoning that drives these adjustments, applying pharmacological knowledge and patient-specific data to recommend protocol modifications. As a core subdomain, it represents the sophisticated analytical capability that distinguishes expert HRT management from static prescribing.

## Domain Model

### Aggregates

The OptimizationRecommendation aggregate is the primary aggregate root, encapsulating a proposed protocol modification with its supporting evidence, clinical rationale, and review status. It enforces the invariant that every recommendation must include at least one supporting evidence reference and a clinical rationale statement.

### Key Entities

The OptimizationRecommendation entity manages the recommendation lifecycle from proposal through prescriber review to acceptance, rejection, or implementation. Its status transitions are tracked for audit and clinical learning purposes.

### Key Value Objects

DoseAdjustment specifies the current dose, proposed dose, direction, and magnitude of a dosing change. TitrationDirection indicates whether to increase, decrease, maintain, or discontinue. EvidenceSummary captures the data sources and confidence level supporting the recommendation.

## Dose Titration

### Titration Principles

Dose titration in HRT follows the principle of using the lowest effective dose to achieve therapeutic targets while minimizing risk. The TitrationCalculationService applies algorithms that consider the current hormone level relative to the therapeutic target, the trend direction and velocity from sequential lab results, the presence and severity of side effects, the elapsed time since the last dose adjustment (allowing sufficient time for pharmacokinetic steady state), and patient-reported symptom response.

### Titration Algorithms

Up-titration is recommended when hormone levels remain below the therapeutic target range after sufficient time at the current dose and the patient continues to report significant symptoms without dose-limiting side effects. The typical adjustment increment is 25-50% of the current dose for estrogen and testosterone, with reassessment after 4-8 weeks.

Down-titration is recommended when hormone levels exceed the therapeutic target range, when dose-dependent side effects emerge, or when the patient achieves symptom resolution and may benefit from dose reduction to the minimum effective level. Decrements are typically 25% of the current dose to avoid rapid hormonal changes.

Maintenance is recommended when hormone levels are within the therapeutic target range, symptoms are adequately controlled, and no dose-limiting side effects are present. The current dose is maintained with continued monitoring at standard intervals.

## Formulation Adjustment

The FormulationSwitchingService evaluates whether a patient would benefit from changing the hormone formulation or delivery method while maintaining the therapeutic intent. Common scenarios include switching from oral to transdermal estrogen to reduce thromboembolism risk in patients with risk factors or to improve hepatic safety. Switching from injectable to pellet testosterone for more stable hormone levels and improved adherence. Changing from synthetic progestins to micronized progesterone for improved side effect profile. Adjusting from systemic to local estrogen therapy when symptoms are primarily urogenital.

## Combination Therapy Planning

When single-hormone therapy is insufficient, the optimization context evaluates combination approaches. Common combinations include adding testosterone to estrogen-progesterone therapy for persistent fatigue, low libido, or cognitive complaints. Adding DHEA supplementation for patients with adrenal insufficiency. Adjusting the estrogen-progesterone ratio in combined regimens to balance endometrial protection with progesterone-related side effects.

## Evidence Synthesis

Each optimization recommendation is supported by an evidence synthesis that documents the specific data points informing the recommendation. Evidence sources include laboratory trend data (direction, velocity, distance from target), adverse event history (type, frequency, severity, relationship to dosing), outcomes measurements (symptom scores, quality of life changes), treatment duration and history of previous adjustments, and clinical guideline recommendations for the patient's specific situation.

## Recommendation Workflow

The optimization recommendation workflow begins with automated or clinician-triggered analysis. The system evaluates current treatment data against optimization criteria. When adjustment is indicated, an OptimizationRecommendation is generated with specific proposed changes, supporting evidence, and clinical rationale. The recommendation is published as an OptimizationRecommended event to the Hormone Protocol Context, where the prescribing clinician reviews and accepts, modifies, or rejects the recommendation. Implemented recommendations are tracked for outcome evaluation.

## Domain Events

The context publishes OptimizationRecommended when analysis produces a protocol change proposal and TitrationCompleted when a titration sequence reaches its target or is concluded.

## Context Relationships

The Treatment Optimization Context is downstream to Lab Monitoring (consuming trend data through an open host service), Side Effect Management (conforming to its risk assessments), and Outcomes Tracking (consuming efficacy data through published language). It is upstream to the Hormone Protocol Context, supplying advisory recommendations. It maintains an ACL against Lab Monitoring to translate raw results into optimization-specific representations.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Santen, R.J., et al. "Postmenopausal Hormone Therapy: An Endocrine Society Scientific Statement." JCEM, 2010.
- Bhasin, S., et al. "Testosterone Therapy in Men with Hypogonadism." JCEM, 2018.
