# Value Objects in the Rheumatology Domain

## Overview

Value objects are immutable domain objects defined by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal. In the rheumatology domain, value objects represent clinical measurements, scores, classifications, and standards that are intrinsically defined by their content. Value objects are ideal for representing rheumatology concepts that are measured, calculated, or classified rather than tracked over time as individual instances.

## Clinical Assessment Value Objects

### DAS28Score

The DAS28Score value object encapsulates the Disease Activity Score using 28 joints. It contains the tender joint count (0-28), swollen joint count (0-28), ESR or CRP value, patient global assessment (0-100 mm VAS), the calculated composite score, and the interpretation category (remission below 2.6, low 2.6-3.2, moderate 3.2-5.1, high above 5.1). The calculation formula applies square root transformations and natural logarithms. The value object is immutable once calculated and validates that all components are within acceptable ranges.

### SerologyResult

The SerologyResult value object represents a single serologic test result. It contains the test type (RF, anti-CCP, ANA, complement C3/C4), the numeric value with units, the reference range, the positivity determination (positive, negative, equivocal), and for ANA tests, the titer (1:40, 1:80, 1:160, etc.) and pattern (homogeneous, speckled, nucleolar, centromere). Two SerologyResult objects with identical test type, value, and interpretation are considered equal regardless of when they were produced.

### InflammatoryMarker

The InflammatoryMarker value object captures ESR or CRP results. It contains the marker type, numeric value, units (mm/hr for ESR, mg/L or mg/dL for CRP), reference range, and elevation status (normal, mildly elevated, significantly elevated). This object is used as a component within DAS28Score calculations and as a standalone clinical indicator.

### JointCount

The JointCount value object represents a set of joint examination findings. It contains the examination scheme (28-joint or 66/68-joint), individual joint statuses (tender/not-tender, swollen/not-swollen), computed tender joint count, and computed swollen joint count. It validates that the number of examined joints matches the specified scheme.

## Autoimmune Disease Value Objects

### ClassificationCriteria

The ClassificationCriteria value object encapsulates the scoring of disease classification criteria. For RA, it contains the ACR/EULAR 2010 criteria components: joint involvement score (0-5), serology score (0-3), acute phase reactant score (0-1), and duration score (0-1), with a total score and classification determination (classified if total is 6 or greater). For SLE, it contains SLICC/ACR criteria component scores. The value object is immutable and validates that all required components are scored.

### DiseaseActivityLevel

The DiseaseActivityLevel value object represents the interpreted level of disease activity. It contains the disease type, the scoring instrument used, the raw score, and the interpreted level (remission, low, moderate, high). Thresholds are disease-specific and instrument-specific. This value object enables comparison across assessments without exposing the details of the underlying scoring instrument.

## Joint Disease Value Objects

### CrystalAnalysis

The CrystalAnalysis value object represents synovial fluid crystal analysis results. It contains the crystal type identified (monosodium urate, calcium pyrophosphate, none), crystal morphology (needle-shaped, rhomboid), birefringence characteristics (strongly negative for urate, weakly positive for CPP), white blood cell count, and differential. This object is critical for distinguishing gout from pseudogout and from septic arthritis.

### RadiographicGrade

The RadiographicGrade value object captures imaging-based joint damage assessment. It contains the grading system used (Kellgren-Lawrence for OA, Sharp/van der Heijde for RA), the score, the date of imaging, and the joints assessed. For Sharp/van der Heijde scoring, it includes separate erosion and joint space narrowing scores.

## Biologic Therapy Value Objects

### InfusionProtocol

The InfusionProtocol value object defines standardized infusion parameters. It contains the drug, dose calculation method (weight-based or fixed), infusion duration, rate escalation schedule, pre-medication regimen (acetaminophen, diphenhydramine, methylprednisolone), and post-infusion observation period. Two protocols with identical parameters are considered equal.

### SafetyScreening

The SafetyScreening value object captures pre-treatment safety assessment results. It contains tuberculosis screening results (PPD or QuantiFERON), hepatitis B surface antigen and core antibody status, hepatitis C antibody status, vaccination status (pneumococcal, influenza, shingles), and overall clearance determination (cleared, not-cleared with reason).

## Pain Management Value Objects

### PainLevel

The PainLevel value object represents a pain assessment. It contains the assessment scale used (VAS, NRS, McGill), the numeric score, pain location, pain character (aching, sharp, burning, throbbing), and temporal pattern (constant, intermittent, morning stiffness). Two pain assessments with identical attributes are considered equal.

## Outcomes Tracking Value Objects

### HAQDIScore

The HAQDIScore value object represents the Health Assessment Questionnaire Disability Index. It contains scores for eight functional domains (dressing, rising, eating, walking, hygiene, reach, grip, activities), the use of aids and devices, the need for assistance from others, and the computed overall HAQ-DI score (0-3 scale). The value object validates that all domains are scored.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5.
- Prevoo, M.L., et al. "Modified Disease Activity Scores That Include Twenty-Eight-Joint Counts." Arthritis & Rheumatism, 1995.
