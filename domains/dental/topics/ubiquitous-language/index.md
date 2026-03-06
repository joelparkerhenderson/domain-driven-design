# Ubiquitous Language - Dental Domain

## Overview

Ubiquitous language is the shared vocabulary that dental professionals, software developers, and practice stakeholders use consistently when discussing the dental domain model. In a dental practice system, precise terminology is critical because dental professionals rely on standardized clinical language to communicate diagnoses, treatment plans, and procedures. The ubiquitous language bridges the gap between clinical dental terminology and software modeling concepts.

## Importance in the Dental Domain

Dental practice involves highly specific terminology that carries precise clinical meaning. Misunderstanding terms like "restoration" (a procedure to repair tooth structure) versus "rehabilitation" (comprehensive reconstruction of oral function) can lead to incorrect system behavior. The ubiquitous language ensures that when a dentist says "Class II composite," the software system models exactly the same concept: a tooth-colored filling placed on the occlusal and proximal surfaces of a posterior tooth.

The language must be consistent within each bounded context while acknowledging that certain terms carry different meanings across contexts. For example, "recall" in the Clinical Examination Context refers to the clinical need for periodic re-examination, while in the Practice Management Context it refers to the administrative process of contacting patients to schedule those appointments.

## Language Categories

### Clinical Examination Terms

Terms related to diagnostic activities: oral examination, radiographic interpretation, dental charting, caries classification, periodontal probing, occlusal analysis, and diagnostic imaging. These terms are authoritative within the Clinical Examination Context and are communicated as published language to downstream contexts.

### Treatment Planning Terms

Terms related to care planning: treatment plan, treatment sequencing, case presentation, informed consent, cost estimation, treatment alternative, and phased treatment. These terms belong to the Treatment Planning Context and reflect the decision-making process between clinician and patient.

### Restorative Terms

Terms related to restorative procedures: direct restoration, indirect restoration, amalgam, composite resin, crown preparation, impression, cementation, endodontic access, obturation, and implant osseointegration. These terms are specific to the Restorative Services Context.

### Orthodontic Terms

Terms related to orthodontic treatment: malocclusion, Angle classification, cephalometric analysis, bracket placement, archwire sequencing, aligner staging, tooth movement, retention, and relapse. These terms are specific to the Orthodontic Management Context.

### Periodontal Terms

Terms related to periodontal care: probing depth, clinical attachment level, bleeding on probing, scaling, root planing, periodontal maintenance, disease staging, disease grading, and bone loss. These terms are specific to the Periodontal Care Context.

### Practice Management Terms

Terms related to administrative operations: appointment, scheduling, insurance verification, eligibility, benefits, claim, CDT code, patient recall, accounts receivable, and copayment. These terms are specific to the Practice Management Context.

## Language Governance

The ubiquitous language for the dental domain is governed by the following rules:

1. New terms are added only after agreement between dental domain experts and the development team.
2. Each term has a single authoritative definition within its bounded context.
3. When terms are shared across contexts, the context map documents how the term translates between contexts.
4. Clinical terms align with ADA Current Dental Terminology (CDT) definitions where applicable.
5. The language is documented in the ubiquitous-language.md file and referenced consistently across all topic documentation.

## Language Evolution

The dental ubiquitous language evolves as clinical practice advances and new procedures, materials, and technologies emerge. Digital impression systems, CAD/CAM restorations, and cone-beam computed tomography (CBCT) have introduced new terms that must be incorporated into the domain model. The language governance process ensures that new terms are evaluated for their impact on existing bounded contexts before adoption.

## Common Ambiguities to Resolve

Several dental terms require explicit disambiguation in the ubiquitous language:

- "Chart" may refer to the dental charting diagram or the entire patient record. In this domain, "dental chart" refers specifically to the tooth-by-tooth condition record.
- "Cleaning" is a patient-facing term that may correspond to prophylaxis (preventive) or scaling and root planing (therapeutic) in clinical terminology.
- "Filling" is a patient-facing term that corresponds to "direct restoration" in clinical language, with specific subtypes (amalgam, composite, glass ionomer).

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 2 on ubiquitous language.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 1 on developing the ubiquitous language.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 2 on discovering domain knowledge.
- American Dental Association. CDT: Current Dental Terminology. ADA, updated annually.
