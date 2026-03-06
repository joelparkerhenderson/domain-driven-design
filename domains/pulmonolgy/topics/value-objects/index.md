# Value Objects in the Pulmonolgy Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal. In the pulmonolgy domain, value objects represent clinical measurements, classification scores, physiological parameters, and other concepts that are meaningful for their content rather than their identity.

Pulmonolgy is particularly rich in value objects because respiratory medicine relies heavily on quantitative measurements, standardized scores, and classification results. A spirometry result, an oxygen saturation reading, an AHI score, and a GOLD stage classification are all value objects: their meaning comes from their values, not from any notion of persistent identity.

## Value Objects by Bounded Context

### Pulmonary Assessment Context

**SpirometryResult**: Encapsulates FEV1 (liters), FVC (liters), FEV1/FVC ratio, and percent predicted values. Includes quality grade (A through F) based on ATS/ERS acceptability and reproducibility criteria. Equality is determined by all component values matching.

**LungVolumeResult**: Captures total lung capacity (TLC), residual volume (RV), functional residual capacity (FRC), and RV/TLC ratio with percent predicted values. Used to classify restrictive ventilatory defects.

**SymptomSeverityScore**: Represents a standardized symptom assessment using validated instruments such as the modified Medical Research Council (mMRC) dyspnea scale or the COPD Assessment Test (CAT) score. Includes the instrument name, raw score, and severity category.

**ImagingFinding**: Captures a structured chest imaging finding including location (lobe, zone), finding type (opacity, nodule, effusion, consolidation), size measurements, and clinical significance assessment. Multiple imaging findings compose an imaging interpretation.

### Respiratory Diagnostics Context

**DLCOResult**: Encapsulates diffusing capacity measurement including raw DLCO, adjusted DLCO (corrected for hemoglobin and altitude), and percent predicted. Includes measurement quality indicators.

**BronchoscopyFinding**: Represents a finding observed during bronchoscopy including anatomical location, finding type (mucosal abnormality, mass, stenosis, foreign body), and visual description.

**BiopsyResult**: Captures pathological analysis results including tissue type, histological findings, and diagnostic classification. Represents the pathologist's interpretation as a structured value.

**PleuraFluidAnalysis**: Encapsulates the laboratory analysis of pleural fluid including appearance, cell count, protein level, LDH, glucose, pH, and classification as transudative or exudative using Light's criteria.

### Chronic Disease Management Context

**GOLDStage**: Represents the Global Initiative for Chronic Obstructive Lung Disease severity classification. Contains the spirometric grade (1-4), symptom group (A-E), and exacerbation risk category. This value object encodes the GOLD classification algorithm.

**GINAStep**: Represents the Global Initiative for Asthma treatment step (1-5) based on symptom control assessment and risk factor evaluation. Contains current control level and recommended treatment step.

**ExacerbationSeverity**: Classifies an exacerbation as mild (managed with short-acting bronchodilators), moderate (requires systemic corticosteroids or antibiotics), or severe (requires hospitalization or emergency department visit).

**ActionPlanThreshold**: Defines the clinical thresholds for a patient's self-management action plan, including green zone (stable), yellow zone (worsening), and red zone (emergency) criteria based on PEF, symptom scores, or SpO2 values.

### Critical Care Context

**VentilatorSettings**: Captures the complete ventilator parameter set including mode (volume control, pressure control, pressure support, SIMV), tidal volume, respiratory rate, PEEP, FiO2, inspiratory time, and trigger sensitivity. Immutable for a given point in time; setting changes create new value objects.

**ArterialBloodGasResult**: Encapsulates ABG values including pH, PaCO2, PaO2, HCO3, base excess, SaO2, and PaO2/FiO2 ratio. Includes the FiO2 at time of collection for context.

**WeaningReadinessScore**: Represents the assessment of a patient's readiness for ventilator weaning using standardized criteria including rapid shallow breathing index (RSBI), maximum inspiratory pressure (MIP), vital capacity, and clinical stability indicators.

**BerlinCriteria**: Encapsulates the ARDS severity classification using the Berlin definition: timing (acute onset within one week), PaO2/FiO2 ratio categories (mild 200-300, moderate 100-200, severe less than 100), and bilateral opacity confirmation.

### Sleep Medicine Context

**AHIScore**: Represents the apnea-hypopnea index including the total AHI value, obstructive AHI, central AHI, and severity classification (normal less than 5, mild 5-14, moderate 15-29, severe 30 or greater).

**SleepStageDistribution**: Captures the percentage of total sleep time spent in each sleep stage (N1, N2, N3, REM) and wake after sleep onset (WASO). Used for assessing sleep architecture quality.

**OxygenDesaturationIndex**: Represents the number of oxygen desaturation events per hour of sleep, with the desaturation threshold specified (typically 3% or 4% drop from baseline).

**CPAPComplianceRecord**: Captures a period's compliance data including days used, average hours per night, percentage of nights with four or more hours of use, and 90th percentile pressure.

### Pulmonary Rehabilitation Context

**SixMinuteWalkDistance**: Encapsulates the distance walked in meters during a six-minute walk test, including supplemental oxygen use, rest stops taken, pre- and post-test SpO2 and heart rate, and Borg dyspnea and fatigue scores.

**ExercisePrescription**: Defines exercise parameters including modality (treadmill, cycle, upper extremity), intensity target (percentage of peak work rate or target heart rate), duration in minutes, and frequency per week.

**DyspneaScore**: Represents a standardized dyspnea measurement using the Borg CR-10 scale or the modified Borg scale, capturing the numerical score and associated descriptive anchor.

**QualityOfLifeScore**: Captures a validated quality of life assessment such as the St. George's Respiratory Questionnaire (SGRQ) total score and component scores (symptoms, activity, impact).

## Value Object Design Principles

Value objects in the pulmonolgy domain follow these principles:

- **Immutability**: Once created, a value object's attributes cannot change. A new SpO2 reading creates a new value object rather than modifying an existing one.

- **Self-Validation**: Value objects validate their own internal consistency at creation. An AHI score validates that the severity classification matches the numerical value.

- **Domain Logic Encapsulation**: Value objects contain domain logic relevant to their content. A GOLDStage value object can determine recommended pharmacological therapy based on its classification.

- **Measurement Context**: Clinical measurement value objects include the context needed for interpretation, such as reference ranges, measurement conditions, and quality indicators.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects, immutability, and equality semantics.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value object implementation strategies and patterns.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 4 on value object design and when to prefer them over entities.
