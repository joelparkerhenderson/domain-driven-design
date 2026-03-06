# Clinical Integration Context - Sleep Health Metrics

## Overview

The Clinical Integration Context manages bidirectional data exchange between the sleep health metrics domain and external clinical information systems. It translates internal domain models into standardized clinical data formats, generates sleep study reports, maps domain concepts to FHIR R4 resources, manages clinical referral workflows, and handles patient demographic synchronization with electronic health records (EHRs). This context sits downstream of all other bounded contexts and uses anti-corruption layers to contain external system complexity.

## Scope and Boundaries

This context owns all interactions with external clinical systems. No other bounded context communicates directly with EHRs, FHIR servers, insurance platforms, or health information exchanges. This isolation ensures that external data model changes, API version updates, and vendor-specific quirks do not propagate into the core domain logic.

The context consumes data from all five other bounded contexts: sleep study recordings (Sleep Data Collection), scored data and architecture (Sleep Stage Analysis), quality metrics and scores (Sleep Quality Assessment), circadian profiles (Circadian Rhythm), and intervention plans with adherence data (Intervention Tracking).

## FHIR R4 Resource Mapping

The context maps internal domain objects to HL7 FHIR R4 resources for interoperable data exchange:

- **Patient**: Maps to FHIR Patient resource with demographics, identifiers, and contact information.
- **SleepStudy**: Maps to FHIR DiagnosticReport resource (category: sleep study) with references to component Observation resources and the interpreting practitioner.
- **Sleep Quality Metrics**: Each metric (sleep efficiency, latency, WASO, TST, AHI, arousal index) maps to a FHIR Observation resource with appropriate LOINC coding, value, unit, reference range, and interpretation.
- **PSQI and ESS Scores**: Map to FHIR QuestionnaireResponse (raw responses) and Observation (computed scores) resources.
- **Sleep Disorders**: Diagnosed conditions (obstructive sleep apnea, insomnia, circadian rhythm sleep-wake disorders) map to FHIR Condition resources with ICD-10-CM and ICSD-3 coding.
- **Interventions**: CBT-I treatment plans map to FHIR CarePlan resources. CPAP therapy maps to FHIR DeviceUseStatement. Medications map to FHIR MedicationRequest resources.
- **Clinicians**: Map to FHIR Practitioner and PractitionerRole resources.
- **Referrals**: Map to FHIR ServiceRequest resources with appropriate clinical context.

## Sleep Study Report Generation

The context generates standardized clinical reports from sleep study data. A complete sleep study report includes:

- Patient demographics and clinical indication for the study.
- Study type and recording parameters (channels, duration, technologist).
- Sleep architecture summary (TST, sleep efficiency, latency, WASO, stage percentages).
- Hypnogram.
- Respiratory event summary (AHI, RDI, event breakdown by type and position, oxygen desaturation data).
- Arousal summary (index, cause distribution).
- Periodic limb movement summary (index, with and without arousal).
- ECG findings (average heart rate, arrhythmia detection).
- Clinical impression and diagnosis (using ICSD-3 codes).
- Recommendations (follow-up studies, treatment initiation, referrals).
- Interpreting physician signature.

The report format follows AASM clinical practice guidelines for sleep study interpretation and reporting.

## EHR Integration Patterns

The context supports multiple EHR integration approaches:

- **FHIR REST API**: Create, read, update, and search operations against FHIR servers for real-time data exchange.
- **HL7 v2 Messaging**: ORU (observation result) and ORM (order) messages for legacy EHR interfaces.
- **CDA Documents**: Clinical Document Architecture documents for health information exchange (HIE) participation.
- **Bulk FHIR Export**: Large-scale data extraction for population health analysis and research.

## Referral Workflow Management

The context manages clinical referral workflows for sleep evaluation:

- Inbound referrals from primary care or specialty providers requesting sleep evaluation.
- Outbound referrals to sleep specialists, pulmonologists, neurologists, or dentists (for oral appliance therapy).
- Referral status tracking (received, accepted, scheduled, completed, cancelled).
- Clinical context transmission (reason for referral, relevant history, previous sleep study results).

## Anti-Corruption Layer

The Clinical Integration Context maintains anti-corruption layers at each external system boundary:

- EHR-specific ACLs handle vendor differences (Epic FHIR extensions, Cerner proprietary fields, Meditech interface variations).
- Insurance platform ACLs translate between domain adherence data and payer-specific compliance definitions.
- Device manufacturer ACLs translate between proprietary device data download formats and domain canonical representations.

## Key Aggregates

The **ClinicalReport** aggregate assembles a complete sleep study report with references to source data across contexts. The **FHIRResourceMapping** entity tracks correspondence between internal and external resource representations.

## Domain Events Published

- **ClinicalReportGenerated**: Emitted when a report is finalized.
- **FHIRResourcePublished**: Emitted when data is successfully published to a FHIR server.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- HL7 International. (2019). *FHIR R4 Specification*. https://hl7.org/fhir/R4/
- Kapur, V. K. et al. (2017). Clinical Practice Guideline for Diagnostic Testing for Adult Obstructive Sleep Apnea. *Journal of Clinical Sleep Medicine*, 13(3), 479-504.
- LOINC Committee. (2023). *LOINC Codes for Sleep Medicine*. https://loinc.org/
