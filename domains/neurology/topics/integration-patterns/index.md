# Integration Patterns - Neurology Domain

## Overview

Integration patterns define how bounded contexts within the neurology domain interact with each other and with external healthcare systems. These patterns ensure that each bounded context maintains its model integrity while enabling the data exchange necessary for cohesive patient care.

The neurology domain uses multiple integration patterns from DDD, selected based on the relationship type between the interacting contexts and the degree of model alignment between them.

## Internal Integration Patterns

### Shared Kernel

**Clinical Assessment <-> Neurodegenerative Disease**

These two contexts share a kernel of common clinical concepts. The shared kernel includes:

- Patient clinical profile: demographics, comorbidities, medication list.
- Cognitive assessment scores: MMSE and MoCA results with scoring details.
- Baseline neurological status: a standardized summary of the patient's neurological function.

Both teams share ownership of the kernel model. Changes to shared concepts require agreement from both the Clinical Assessment and Neurodegenerative Disease teams. The shared kernel is kept intentionally small to minimize coordination overhead.

Rules for the shared kernel:
- Only concepts genuinely needed by both contexts belong in the kernel.
- The kernel is versioned separately from either bounded context.
- Changes require a formal review process involving both teams.
- The kernel includes only data transfer objects and interfaces, not domain logic.

### Customer-Supplier

**Clinical Assessment (Customer) <-> Neuroimaging (Supplier)**

The Clinical Assessment Context orders imaging studies and consumes results. The Neuroimaging Context defines the integration interface:

- The supplier (Neuroimaging) publishes a contract defining what clinical data must accompany an imaging order and what format the results will take.
- The customer (Clinical Assessment) conforms to the ordering interface but can influence its evolution through a formal change request process.
- The supplier commits to maintaining backward compatibility of the results interface.

**Epilepsy Management (Customer) <-> Neuroimaging (Supplier)**

Epilepsy Management orders specialized imaging (MRI epilepsy protocol, PET, SPECT) and consumes results with epilepsy-specific interpretation.

**Neuromuscular Disorders (Customer) <-> Neuroimaging (Supplier)**

Neuromuscular Disorders orders electrodiagnostic studies (EMG/NCS) and consumes structured results.

### Conformist

**Neurodegenerative Disease (Conformist) <-> Neuroimaging (Upstream)**

The Neurodegenerative Disease Context conforms entirely to the Neuroimaging Context's model for imaging biomarkers. When Neuroimaging defines how hippocampal volume or amyloid PET SUVr values are reported, the Neurodegenerative Disease Context adopts that format without translation.

This pattern is appropriate because:
- Imaging biomarkers are defined by imaging standards, not disease management standards.
- The Neurodegenerative Disease Context has no leverage to change imaging report formats.
- The cost of translation outweighs the benefit of maintaining a separate model.

### Partnership

**Neurorehabilitation <-> Neurodegenerative Disease**

These contexts maintain a bidirectional partnership for patients with progressive conditions requiring ongoing rehabilitation. Neither context dominates the relationship:

- Neurodegenerative Disease provides disease progression milestones that affect rehabilitation planning.
- Neurorehabilitation provides functional outcome data that informs disease staging and treatment decisions.
- Both teams coordinate on shared patients through regular communication and aligned release schedules.

## External Integration Patterns

### Anti-Corruption Layer

Anti-corruption layers protect neurology bounded contexts from external system models. Key ACL implementations:

**Laboratory System ACL**: translates generic lab results into neurology-specific biomarker concepts (CSF analysis, genetic testing, therapeutic drug levels).

**Hospital ADT ACL**: translates admission/discharge/transfer events into neurology-specific care concepts (neuro-ICU admission, EMU admission, rehabilitation transfer).

**Pharmacy System ACL**: translates medication orders and formulary data between generic pharmacy models and neurology-specific therapeutic concepts.

**External EHR ACL**: translates clinical documents from external health systems into neurology domain concepts for referral processing and shared care.

### Published Language

The neurology domain uses published languages for standardized communication:

**HL7 FHIR Resources**
- Patient, Encounter, Condition, Observation, DiagnosticReport, ImagingStudy, MedicationRequest, CarePlan.
- FHIR profiles define neurology-specific constraints on generic resources.
- FHIR extensions capture neurology concepts not represented in the base standard.

**DICOM Standard**
- DICOM objects for imaging data exchange.
- DICOM worklist for imaging workflow integration.
- DICOM Structured Reporting for machine-readable imaging reports.

**Clinical Terminology Standards**
- ICD-10/ICD-11 for diagnostic coding.
- CPT for procedure coding.
- RxNorm for medication coding.
- LOINC for laboratory observation coding.
- SNOMED CT for clinical concept representation.

### Open Host Service

**Neuroimaging Context DICOM Service**

The Neuroimaging Context exposes a DICOM-compliant service interface:
- C-FIND: query for imaging studies by patient, date, modality.
- C-MOVE: retrieve imaging studies to specified destinations.
- C-STORE: receive imaging studies from acquisition devices.
- WADO-RS: web-based access to DICOM objects.

This open host service allows any DICOM-compliant system to interact with the neurology domain's imaging data without custom integration.

## Integration Testing

Integration between bounded contexts requires testing at multiple levels:

- Contract tests verify that supplier contexts maintain their published interfaces.
- Consumer-driven contract tests verify that consumer expectations are met.
- Integration tests verify end-to-end event flow across context boundaries.
- Compatibility tests verify that shared kernel changes do not break either consuming context.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context mapping patterns.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context map integration patterns.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
- HL7 International. FHIR R4 Specification.
- DICOM Standards Committee. DICOM Standard.
