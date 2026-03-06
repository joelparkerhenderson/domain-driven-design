# Dentistry Domain

## Introduction

This domain applies Domain-Driven Design (DDD) to dentistry and oral healthcare systems. Dentistry encompasses the diagnosis, prevention, and treatment of conditions affecting the teeth, gums, jaw, and oral cavity. The field spans multiple specialties including restorative dentistry, endodontics, periodontics, prosthodontics, orthodontics, and oral surgery, each with distinct clinical workflows, terminology, and treatment modalities that must be accurately modeled.

## Bounded Contexts

1. **Clinical Examination** - Manages the patient intake, comprehensive oral examination, dental charting, periodontal probing, occlusal assessment, and diagnostic evaluation workflows. This context captures the baseline clinical findings that inform all subsequent treatment decisions and serves as the foundational record of the patient's oral health status.

2. **Treatment Planning** - Handles the creation, sequencing, phasing, costing, and authorization of multi-stage treatment plans. This context manages priority classification of treatment needs, patient consent for proposed interventions, insurance pre-authorization, and coordination of referrals to dental specialists.

3. **Dental Procedures** - Governs the execution and documentation of clinical procedures across all dental specialties, including restorations, root canal therapy, extractions, crown and bridge fabrication, implant placement, orthodontic adjustments, and periodontal surgery. This context records procedure details, materials used, and clinical outcomes.

4. **Preventive Care** - Tracks preventive interventions including professional prophylaxis, fluoride application, pit and fissure sealants, oral hygiene instruction, dietary counseling, and risk-based recall scheduling. This context supports population-level prevention strategies and individual patient risk management.

5. **Dental Imaging** - Manages the ordering, acquisition, storage, interpretation, and reporting of dental radiographs and other imaging modalities, including periapical, bitewing, panoramic, cephalometric, and cone-beam computed tomography (CBCT) images.

6. **Periodontal Management** - Handles the specialized assessment, classification, and treatment of periodontal and peri-implant diseases, including probing depth recording, attachment level measurement, disease staging and grading, and non-surgical and surgical periodontal therapy.

## Strategic Design

- **Subdomains** classify areas by strategic importance, with clinical examination and treatment planning as core subdomains and procedures, preventive care, imaging, and periodontal management as supporting subdomains.
- **Context Map** defines relationships between bounded contexts, such as the upstream dependency of Treatment Planning on Clinical Examination.
- **Anti-Corruption Layers** protect bounded contexts from external insurance, laboratory, and radiology systems.

## Tactical Design

- **Aggregates** enforce consistency boundaries, such as the Dental Chart aggregate that groups tooth records, surface conditions, and existing restorations.
- **Entities** represent domain objects with unique identity, such as Patient, Tooth, Treatment Plan, and Procedure Record.
- **Value Objects** capture immutable measurements and types, such as Tooth Number, Surface Designation, Probing Depth, and Material Type.
- **Domain Events** signal significant occurrences, such as ExaminationCompleted, TreatmentPlanApproved, ProcedurePerformed, and RecallScheduled.
- **Repositories** abstract persistence of dental charts, treatment plans, and procedure records.
- **Domain Services** encapsulate cross-aggregate operations, such as calculating treatment plan cost estimates across multiple procedure codes.

## References

- Evans, Eric. *Domain-Driven Design*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- American Dental Association. *CDT: Code on Dental Procedures and Nomenclature*. ADA, published annually.
- Summitt, James B., et al. *Fundamentals of Operative Dentistry: A Contemporary Approach*. 4th ed. Quintessence, 2013.
- Newman, Michael G., et al. *Newman and Carranza's Clinical Periodontology*. 13th ed. Elsevier, 2019.
