# Clinical Assessment Context - Otolaryngology Domain

## Overview

The Clinical Assessment Context is the foundational bounded context in the otolaryngology domain. It encompasses all diagnostic and evaluative activities performed during ENT clinical encounters, including head and neck physical examination, nasal and laryngeal endoscopy, audiometric screening, and diagnostic imaging interpretation. This context serves as the primary upstream context, producing diagnostic information consumed by all other bounded contexts.

## Subdomain Classification

Clinical Assessment is a core subdomain. The ability to model complex diagnostic workflows and correlate multi-modal clinical findings differentiates this system from generic clinical documentation tools. The domain model must capture the nuanced clinical reasoning that ENT specialists apply when synthesizing physical examination findings, endoscopic observations, audiometric data, and imaging results into coherent diagnostic impressions.

## Ubiquitous Language

- **ENT Encounter**: A clinical visit between an otolaryngology provider and a patient, serving as the primary aggregate root.
- **Head and Neck Examination**: Systematic physical assessment of ears, nose, throat, oral cavity, neck, and related structures.
- **Endoscopic Report**: Structured documentation of findings observed during nasal or laryngeal endoscopy.
- **Audiometric Screening**: Initial hearing assessment performed in the general ENT clinic setting.
- **Clinical Impression**: The provider's diagnostic interpretation synthesizing all encounter findings.
- **Examination Finding**: A discrete observation recorded during physical examination with anatomical site, finding type, and severity.

## Aggregate: ENT Encounter

The ENT Encounter is the primary aggregate root. It maintains consistency across all clinical activities performed during a single patient visit.

### Entities Within the Aggregate

- **Examination Finding**: Records a specific physical finding at an anatomical site with laterality and severity.
- **Endoscopic Report**: Documents nasal or laryngeal endoscopic findings with structured observations.
- **Audiometric Screening Result**: Records screening-level hearing test results.
- **Imaging Study Reference**: Links to imaging studies ordered or reviewed during the encounter.
- **Clinical Impression**: The provider's synthesis of findings into diagnostic conclusions.
- **Referral**: A recommendation for subspecialty evaluation generated from encounter findings.

### Key Value Objects

- **Anatomical Site**: Region, laterality, specific structure within the head and neck.
- **Finding Severity**: Classification of clinical finding severity (normal, mild, moderate, severe).
- **ICD-10 Code**: Diagnosis code associated with clinical impressions.
- **Encounter Status**: Current state (scheduled, in-progress, documentation-complete, signed, amended).

### Invariants

- Every encounter must have an assigned provider and encounter date.
- Examination findings must reference a valid anatomical site.
- Clinical impressions must be supported by at least one documented finding or test result.
- An encounter cannot be signed until all required documentation sections are complete.
- Once signed, an encounter can only be modified through a formal amendment process.

## Domain Events Published

- **EncounterCompleted**: Triggers downstream workflows in subspecialty contexts.
- **EndoscopyPerformed**: Notifies Voice Disorders and Allergy Management of relevant findings.
- **AudiometricScreeningCompleted**: Triggers Hearing Services evaluation when abnormalities are detected.
- **ImagingStudyReviewed**: Notifies Surgical Management of findings relevant to surgical planning.

## Domain Services

- **DiagnosticCorrelationService**: Correlates findings across examination modalities to generate differential diagnoses.
- **ReferralDecisionService**: Analyzes findings against evidence-based referral criteria to recommend subspecialty evaluations.

## Integration Points

### Downstream Consumers

All five other bounded contexts consume Clinical Assessment data through domain events. Each consumer applies its own anti-corruption layer to translate generic assessment findings into context-specific concepts.

### External Systems

- **EHR Integration**: Clinical encounter data is exchanged with EHR systems through HL7 FHIR resources (Encounter, Observation, DiagnosticReport). An anti-corruption layer translates between FHIR's generic observation model and the domain's structured examination finding model.
- **PACS Integration**: Imaging study references connect to external picture archiving systems through an ACL that translates DICOM metadata into domain-specific Imaging Study Reference value objects.
- **Billing Integration**: Encounter completion events are translated into billable encounters with appropriate CPT evaluation and management codes and ICD-10 diagnosis codes.

## Clinical Workflow

1. Patient presents for ENT evaluation (encounter created).
2. Provider performs head and neck examination (examination findings recorded).
3. Provider performs endoscopy if indicated (endoscopic report created).
4. Provider reviews audiometric screening if obtained (screening results recorded).
5. Provider reviews imaging studies if available (imaging references linked).
6. Provider synthesizes findings into clinical impressions (impressions documented).
7. Provider generates referrals if indicated (referral events published).
8. Provider signs encounter (encounter finalized, domain events published).

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Flint, Paul W., et al. *Cummings Otolaryngology: Head and Neck Surgery*. 7th ed., Elsevier, 2020. Part I on general assessment.
- American Academy of Otolaryngology-Head and Neck Surgery. Clinical Practice Guidelines. https://www.entnet.org.
