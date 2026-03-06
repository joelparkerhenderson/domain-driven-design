# Body Health Metrics Standards

## Overview

Body health metrics standards are the established coding systems, data formats, measurement protocols, and clinical terminology standards that inform the design of value objects, published languages, and anti-corruption layers within the body health metrics domain. Adherence to these standards ensures data interoperability with clinical systems, measurement consistency across devices and methodologies, and alignment with evidence-based health assessment practices.

## Clinical Data Standards

### LOINC (Logical Observation Identifiers Names and Codes)

LOINC provides universal codes for identifying medical laboratory observations and clinical measurements. In the body health metrics domain, LOINC codes are used within the Clinical Integration Context to ensure that measurements are unambiguously identified when exchanged with external clinical systems.

Key LOINC codes for body health metrics:

- 29463-7: Body weight
- 8302-2: Body height
- 39156-5: Body mass index (BMI)
- 8280-0: Waist circumference
- 41982-0: Percentage of body fat
- 8277-6: Body surface area
- 29463-7: Body weight (measured)
- 85354-9: Blood pressure panel with all children
- 8867-4: Heart rate
- 8310-5: Body temperature
- 9279-1: Respiratory rate
- 60985-9: Body fat percentage by DEXA
- 73708-0: Lean body mass by DEXA

LOINC codes appear as value object attributes within the Clinical Integration Context, mapped from internal measurement types through the ClinicalCodingService.

### SNOMED CT (Systematized Nomenclature of Medicine - Clinical Terms)

SNOMED CT provides a comprehensive clinical terminology for encoding clinical concepts. It supplements LOINC by providing codes for clinical findings, body structures, and assessment procedures.

Relevant SNOMED CT concepts:

- 27113001: Body weight
- 1153637007: Body height
- 60621009: Body mass index
- 248361005: Body fat distribution
- 165263003: Body mass index finding
- 271649006: Systolic blood pressure
- 271650006: Diastolic blood pressure
- 364075005: Heart rate

### HL7 FHIR (Fast Healthcare Interoperability Resources)

FHIR provides the resource model and exchange protocols for health data interoperability. The Clinical Integration Context uses FHIR R4 resources as its published language for external system communication.

Key FHIR resources:

- **Observation**: The primary resource for body measurements, carrying a LOINC code, value with UCUM unit, reference range, and interpretation
- **DiagnosticReport**: Groups related observations into a comprehensive assessment report (used for composition profiles and fitness assessments)
- **Patient**: Represents the individual whose body health metrics are being tracked
- **RiskAssessment**: Represents composite health scores and wellness index assessments with predicted outcomes
- **Goal**: Represents health improvement objectives with target values and timelines
- **Bundle**: Groups related FHIR resources for batch transmission

### UCUM (Unified Code for Units of Measure)

UCUM provides standardized unit codes used within FHIR Observation resources. All measurement values exchanged through the Clinical Integration Context use UCUM unit codes.

Relevant UCUM codes:

- kg: kilogram (body weight)
- cm: centimeter (height, circumferences)
- %: percent (body fat percentage, hydration level)
- mm[Hg]: millimeters of mercury (blood pressure)
- /min: per minute (heart rate, respiratory rate)
- Cel: degrees Celsius (body temperature)
- mL/kg/min: milliliters per kilogram per minute (VO2 max)
- g/cm2: grams per square centimeter (bone mineral density)
- kcal/d: kilocalories per day (basal metabolic rate)

## Measurement Science Standards

### WHO Anthropometric Standards

The World Health Organization publishes reference standards for anthropometric measurements including BMI classification thresholds, waist circumference risk thresholds, and growth reference charts. These standards inform the classification logic in value objects and scoring algorithms.

- BMI categories: underweight (<18.5), normal (18.5-24.9), overweight (25.0-29.9), obese (>=30.0)
- Waist circumference risk thresholds: elevated risk at >94 cm (males), >80 cm (females)
- Waist-to-hip ratio risk thresholds: elevated risk at >0.90 (males), >0.85 (females)

### ACSM Fitness Assessment Standards

The American College of Sports Medicine publishes standardized fitness assessment protocols, normative databases, and classification criteria. These standards inform the Fitness Assessment Context's protocol implementations, normative comparison data, and fitness classification logic.

- VO2 max normative tables by age and sex
- Body composition classification thresholds
- Muscular fitness assessment protocols and norms
- Flexibility assessment protocols and norms
- Cardiorespiratory fitness classification categories

### ISCD Bone Densitometry Standards

The International Society for Clinical Densitometry publishes standards for bone mineral density measurement and interpretation, including T-score and Z-score calculation methods and osteoporosis classification thresholds.

- T-score classification: normal (>= -1.0), osteopenia (-1.0 to -2.5), osteoporosis (< -2.5)
- Z-score interpretation: below expected range for age (< -2.0)

## Standards Integration in the Domain Model

Standards influence domain model design in several ways:

1. **Value object validation rules** derive from WHO and ACSM classification thresholds
2. **Clinical coding attributes** on value objects use LOINC and SNOMED CT codes
3. **Unit representations** follow UCUM conventions
4. **FHIR resource construction** in the Clinical Integration Context follows HL7 FHIR R4 specifications
5. **Normative comparison databases** in the Fitness Assessment Context align with ACSM published norms
6. **Bone density classifications** follow ISCD T-score interpretation guidelines

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- HL7 FHIR R4 Specification. https://www.hl7.org/fhir/
- LOINC. https://loinc.org/
- SNOMED International. https://www.snomed.org/
- UCUM. https://ucum.org/
- World Health Organization. *Physical Status: The Use and Interpretation of Anthropometry*. WHO Technical Report Series 854, 1995.
- American College of Sports Medicine. *ACSM's Guidelines for Exercise Testing and Prescription*. 11th ed. Wolters Kluwer, 2021.
