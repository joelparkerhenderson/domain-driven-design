# Clinical Assessment Context

## Overview

The Clinical Assessment Context is the diagnostic gateway of the urology domain. It encompasses the systematic evaluation of patients presenting with urological complaints, from initial history and physical examination through advanced diagnostic studies. This bounded context owns the patient's urological diagnostic profile, producing structured clinical assessments that downstream treatment contexts consume to guide therapeutic decisions.

## Scope

This context covers urological history taking, physical examination (including digital rectal examination), symptom scoring instruments (IPSS, AUA Symptom Index, SHIM), uroflowmetry, post-void residual measurement, urodynamic studies (cystometrogram, pressure-flow study, electromyography), cystoscopy, and interpretation of urological imaging (CT urogram, renal ultrasound, MRI prostate with PI-RADS scoring, nuclear medicine renal scans).

## Ubiquitous Language

Within this context, specific terms carry precise meanings. "Workup" refers to the complete diagnostic evaluation for a presenting complaint. "Symptom score" always refers to a validated instrument with defined scoring rules. "Urodynamic study" encompasses the full set of functional bladder tests, not a single measurement. "Imaging finding" is a structured observation translated from radiology reporting into urological diagnostic categories. "Diagnostic conclusion" is the formal output of the workup, representing the assessed condition with supporting evidence.

## Aggregates

The primary aggregate is the DiagnosticWorkup, rooted by a workup identifier and containing all diagnostic components gathered for a specific clinical question. The UrodynamicSession aggregate manages a complete urodynamic testing session with its constituent phases and measurements. Each aggregate enforces its own consistency rules independently.

## Key Entities

The Patient entity maintains the patient's identity and urological history within this context. CystoscopyFinding entities record individual observations with location, appearance, and size. UrodynamicStudy entities capture complete functional evaluations. ImagingResult entities translate radiology reports into structured urological findings.

## Value Objects

IPSSScore captures the seven-question symptom score plus quality of life. UrinaryFlowRate records peak and average flow with voided volume. PostVoidResidual captures the measured retained urine volume. ProstateSizeEstimate records volume in cubic centimeters derived from ultrasound or MRI measurements. PI-RADSScore captures the Prostate Imaging Reporting and Data System classification for MRI findings.

## Domain Events

DiagnosticWorkupCompleted is published when a workup reaches a definitive conclusion, triggering downstream treatment planning. ImagingSuspiciousLesionIdentified is published when imaging reveals a finding requiring urgent follow-up. UrodynamicStudyRecorded is published when urodynamic testing is completed, informing the Incontinence Management and Surgical Management contexts.

## Clinical Workflows

The context supports several diagnostic workflows. The hematuria workup follows AUA guidelines for evaluating blood in the urine based on patient age and risk factors. The LUTS (lower urinary tract symptoms) workup evaluates obstructive and irritative urinary symptoms. The renal mass workup characterizes incidentally discovered kidney lesions. Each workflow defines the required diagnostic components and their sequencing.

## Integration Points

The Clinical Assessment Context integrates upstream with the EHR for patient demographics and medical history. It integrates upstream with radiology systems for imaging reports and with laboratory systems for urine studies and bloodwork. It publishes downstream to the Oncologic Urology Context (suspicious findings), Stone Disease Context (stone identification), Incontinence Management Context (urodynamic results), and Male Reproductive Health Context (hormonal and physical examination findings).

## Anti-Corruption Layer

The ACL between Clinical Assessment and radiology systems translates structured radiology reports (PI-RADS scores, Bosniak classifications, stone measurements) into the context's own diagnostic model. The ACL between Clinical Assessment and the EHR translates generic patient records into urologically-focused patient profiles with relevant history extraction.

## Business Rules

A diagnostic workup cannot produce a conclusion without at least one objective diagnostic finding. Symptom scores must use validated instruments administered according to their defined protocols. Urodynamic studies must include proper artifact identification and quality assurance checks before results are accepted. Imaging interpretations must reference the original radiology report and any discrepancies between urological and radiological interpretation must be documented.

## Relationship to Other Contexts

The Clinical Assessment Context is the primary upstream context in the urology domain. It does not make treatment decisions but provides the diagnostic foundation upon which all treatment contexts build. Its diagnostic profiles are consumed but never modified by downstream contexts. When surgical findings or treatment outcomes require updating the diagnostic profile, the Clinical Assessment Context processes these updates through its own domain logic.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Urological Association. Diagnosis, Evaluation, and Follow-Up of Asymptomatic Microhematuria in Adults. AUA Guideline, 2020.
- Abrams, P. et al. "The Standardisation of Terminology of Lower Urinary Tract Function." Neurourology and Urodynamics, 2002.
