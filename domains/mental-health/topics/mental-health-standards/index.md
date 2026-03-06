# Mental Health Standards

## Overview

Mental health standards are the clinical, terminological, and technical
standards that inform the domain model's design. In Domain-Driven Design,
standards serve as published languages and shape value object design by
providing validated classification systems, measurement frameworks, and
interoperability specifications. The mental health domain relies on a rich
ecosystem of standards spanning diagnostic classification, outcome measurement,
data exchange, and quality reporting.

Understanding these standards is essential for domain modelers because they
define the vocabulary, valid ranges, and structural constraints that the
domain model must respect. A DiagnosticCode value object must conform to
DSM-5 or ICD-11 format rules. An AssessmentScore must fall within the
instrument's validated range. A medication prescription must use NDC or
RxNorm coding.

## Diagnostic Classification Standards

### DSM-5 (Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition)

Published by the American Psychiatric Association (2013), the DSM-5 is the
primary classification system for mental health diagnoses in the United States.
It defines diagnostic criteria, severity specifiers, and course specifiers for
mental disorders. In the domain model, DSM-5 codes appear as DiagnosticCode
value objects within the Clinical Assessment Context and as references in the
Treatment Planning Context.

Key modeling implications: diagnostic codes include a category, a specific
disorder within the category, and optional specifiers (severity, course,
onset). The domain model must support the full code structure, not just the
numeric code.

### ICD-11 (International Classification of Diseases, 11th Revision)

Published by the World Health Organization (2019), ICD-11 provides an
international diagnostic classification that is used for morbidity coding,
mortality statistics, and international clinical communication. ICD-11 codes
are required for insurance billing in many jurisdictions. In the domain model,
ICD-11 codes coexist with DSM-5 codes and the anti-corruption layer handles
mapping between the two systems where they diverge.

## Assessment Instrument Standards

### PHQ-9 (Patient Health Questionnaire-9)

Developed by Kroenke, Spitzer, and Williams (2001), the PHQ-9 is a 9-item
self-report instrument that screens for and measures the severity of major
depressive disorder. Each item is scored 0-3, yielding a total range of 0-27.
Severity bands: minimal (0-4), mild (5-9), moderate (10-14), moderately
severe (15-19), severe (20-27). The domain model's AssessmentScore value
object for PHQ-9 must validate scores within this range and map to the
correct severity band.

### GAD-7 (Generalized Anxiety Disorder-7)

Developed by Spitzer, Kroenke, Williams, and Lowe (2006), the GAD-7 is a
7-item self-report instrument for anxiety severity. Each item is scored 0-3,
yielding a total range of 0-21. Severity bands: minimal (0-4), mild (5-9),
moderate (10-14), severe (15-21).

### C-SSRS (Columbia Suicide Severity Rating Scale)

Developed by Posner et al. (2011), the C-SSRS is a structured interview for
assessing suicidal ideation and behavior. It classifies ideation on a 5-point
intensity scale and categorizes behavior types. Used in the Crisis
Intervention Context for risk assessment.

### WHODAS 2.0 (WHO Disability Assessment Schedule)

Published by the World Health Organization, WHODAS 2.0 measures functional
disability across six domains: cognition, mobility, self-care, getting along,
life activities, and participation. Used in the Outcomes Measurement Context
for functional assessment.

## Data Exchange Standards

### HL7 FHIR (Fast Healthcare Interoperability Resources)

HL7 FHIR provides standardized resource types for clinical data exchange.
Mental health-relevant FHIR resources include: Patient, Condition (for
diagnoses), Observation (for assessment scores), MedicationRequest (for
prescriptions), CarePlan (for treatment plans), and Encounter (for sessions).
Anti-corruption layers translate between FHIR resources and the domain model.

### NCPDP SCRIPT

The National Council for Prescription Drug Programs SCRIPT standard governs
electronic prescribing messages. The Medication Management Context uses
NCPDP SCRIPT for communication with pharmacy systems through an anti-
corruption layer.

### LOINC (Logical Observation Identifiers Names and Codes)

LOINC provides standardized codes for clinical observations and
measurements. Mental health assessment instruments have LOINC codes (e.g.,
LOINC 44249-1 for PHQ-9 total score). The Outcomes Measurement Context
uses LOINC codes to uniquely identify measurement types.

## Terminology Standards

### SNOMED CT (Systematized Nomenclature of Medicine - Clinical Terms)

SNOMED CT provides a comprehensive clinical terminology for describing
clinical findings, procedures, and concepts. It is used as a reference
terminology for mapping between different coding systems and for enabling
semantic interoperability.

### RxNorm

RxNorm provides normalized names for clinical drugs and links to many
drug vocabularies. The Medication Management Context uses RxNorm identifiers
as part of the MedicationIdentifier value object to ensure unambiguous
medication identification.

## Quality Measurement Standards

### HEDIS (Healthcare Effectiveness Data and Information Set)

HEDIS includes behavioral health quality measures such as follow-up after
hospitalization for mental illness, antidepressant medication management,
and metabolic monitoring for children and adolescents on antipsychotics.
The Outcomes Measurement Context produces data for HEDIS reporting.

### NQF (National Quality Forum)

NQF endorses quality measures for mental health care, including depression
remission rates and follow-up after emergency department visits for mental
health conditions.

## References

- American Psychiatric Association. DSM-5. APA Publishing, 2013.
- World Health Organization. ICD-11. WHO, 2019.
- Kroenke, K. et al. "The PHQ-9." Journal of General Internal Medicine, 2001.
- Spitzer, R.L. et al. "The GAD-7." Archives of Internal Medicine, 2006.
- Posner, K. et al. "The C-SSRS." American Journal of Psychiatry, 2011.
- HL7 International. FHIR R4 Specification. https://www.hl7.org/fhir/
- Evans, Eric. Domain-Driven Design. Addison-Wesley, 2003. Chapter 14 on published language.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
