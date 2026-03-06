# Anti-Corruption Layer - Otolaryngology Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from external system concepts, preventing foreign models from leaking into the domain. In the otolaryngology domain, anti-corruption layers are critical at boundaries between bounded contexts and at integration points with external systems such as electronic health records (EHR), laboratory information systems (LIS), billing systems, and imaging archives (PACS).

## Purpose in Otolaryngology

Healthcare systems present unique integration challenges. External systems model clinical data using generic healthcare concepts (encounters, observations, results) that do not capture the specialized semantics of ENT practice. Without anti-corruption layers, the otolaryngology domain model would be polluted by generic EHR data structures, losing the clinical precision that makes the model valuable.

For example, an EHR models a "clinical observation" as a generic key-value pair. The otolaryngology domain needs a rich "endoscopic finding" with anatomical location, finding type, severity, laterality, and clinical significance. The ACL translates between these representations, keeping the domain model pure.

## Inter-Context Anti-Corruption Layers

### Clinical Assessment to Surgical Management

When diagnostic findings from the Clinical Assessment Context are consumed by the Surgical Management Context, an ACL translates them into pre-operative assessment items. A "flexible nasopharyngolaryngoscopy finding" in Clinical Assessment becomes a structured "airway assessment" in Surgical Management, with only the surgically relevant attributes preserved.

Translation responsibilities:
- Map examination findings to pre-operative risk factors
- Translate audiometric results into surgical hearing baselines
- Convert imaging study interpretations into surgical anatomy descriptions

### Clinical Assessment to Hearing Services

Audiometric data from initial screening in Clinical Assessment must be translated for the Hearing Services Context, which requires full audiological precision. The ACL enriches screening-level data with the metadata needed for audiological interpretation (calibration standards, test environment, transducer type).

### Sleep Disorders to Surgical Management

When the Sleep Disorders Context refers a patient for surgical intervention, the ACL translates sleep study data into surgically relevant parameters. The full polysomnography dataset is reduced to the specific metrics that inform surgical decision-making (AHI, oxygen nadir, REM-related events, body position data).

## External System Anti-Corruption Layers

### EHR Integration ACL

The EHR ACL translates between HL7 FHIR resources and otolaryngology domain objects. Key translations include:

- FHIR Encounter to ENT Encounter (adding ENT-specific examination structure)
- FHIR Observation to domain-specific findings (Endoscopic Finding, Audiometric Result)
- FHIR Procedure to Surgical Case (adding ENT surgical workflow states)
- FHIR DiagnosticReport to domain-specific reports (Polysomnography Result, Allergy Test Result)

The ACL ensures that FHIR's generic observation model does not flatten the rich clinical semantics of the otolaryngology domain.

### Laboratory Information System ACL

The Allergy Management Context integrates with laboratory systems for serum-specific IgE results. The ACL translates laboratory result messages (typically HL7 v2 ORU messages) into domain-specific Allergy Test Result value objects, mapping laboratory allergen codes to the domain's Allergen Panel taxonomy.

### Billing System ACL

Each bounded context produces domain events that must be translated into billable encounters. The billing ACL maps domain concepts to CPT procedure codes and ICD-10 diagnosis codes. For example, a "functional endoscopic sinus surgery" domain event maps to CPT code 31254 (maxillary antrostomy) or 31255 (with removal of tissue), depending on the procedure details captured in the domain model.

### PACS Integration ACL

The Clinical Assessment Context integrates with picture archiving and communication systems (PACS) for imaging studies. The ACL translates DICOM metadata into domain-specific Imaging Study value objects, extracting clinically relevant attributes (study type, anatomical region, contrast usage) while leaving the image data in the external system.

## Implementation Principles

1. **Translate at the boundary**: All translation logic resides in the ACL, not in the domain model or the external system adapter.
2. **Domain model purity**: The domain model never references external system data structures (FHIR resources, HL7 messages, DICOM objects).
3. **Bidirectional translation**: ACLs handle both inbound translation (external to domain) and outbound translation (domain to external).
4. **Failure isolation**: If an external system is unavailable, the ACL isolates the failure, preventing it from corrupting the domain model's state.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9 on anti-corruption layer implementation.
- HL7 FHIR. Fast Healthcare Interoperability Resources. https://www.hl7.org/fhir/.
