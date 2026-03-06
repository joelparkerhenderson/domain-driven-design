# Surgical Management Context - Otolaryngology Domain

## Overview

The Surgical Management Context manages the full lifecycle of ENT surgical procedures, from pre-operative evaluation through post-operative follow-up. It covers the broad spectrum of otolaryngologic surgery, including tonsillectomy, adenoidectomy, septoplasty, functional endoscopic sinus surgery (FESS), thyroidectomy, parotidectomy, cochlear implantation, and head and neck oncologic resection. This context captures surgical decision-making, procedural planning, operative documentation, and outcomes tracking.

## Subdomain Classification

Surgical Management is a core subdomain. Each surgical procedure type has unique pre-operative requirements, intra-operative considerations, and post-operative protocols. The domain model must capture the complexity of surgical planning, the relationship between diagnostic findings and surgical indications, and the structured documentation requirements for operative reports and post-operative care.

## Ubiquitous Language

- **Surgical Case**: The primary aggregate representing a planned or completed surgical procedure.
- **Pre-Operative Assessment**: A structured evaluation of surgical readiness including medical clearance, anatomical assessment, and anesthesia planning.
- **Surgical Consent**: Documented informed consent specifying the procedure, risks, benefits, and alternatives.
- **Operative Note**: Structured documentation of the surgical procedure including findings, technique, and specimens.
- **Post-Operative Plan**: A care plan specifying post-operative instructions, medications, activity restrictions, and follow-up schedule.
- **Pathology Result**: Histopathological analysis of surgical specimens with diagnosis and staging if applicable.

## Aggregate: Surgical Case

The Surgical Case is the primary aggregate root, maintaining consistency across the entire surgical lifecycle.

### Entities Within the Aggregate

- **Pre-Operative Assessment**: Records medical clearance, anatomical evaluation, surgical indication justification, and anesthesia plan.
- **Surgical Consent**: Documents the informed consent process including risks discussed, alternatives reviewed, and patient authorization.
- **Operative Note**: Records intra-operative findings, technique, specimens submitted, estimated blood loss, and complications.
- **Post-Operative Plan**: Specifies discharge instructions, medication prescriptions, activity restrictions, and follow-up appointments.
- **Complication Record**: Documents any post-operative complications with classification, management, and outcome.
- **Pathology Result**: Records histopathological diagnosis and staging from submitted specimens.

### Key Value Objects

- **CPT Procedure Code**: Standardized procedure classification with code, description, and modifiers.
- **Surgical Risk Score**: Composite risk including ASA classification, airway assessment, and bleeding risk.
- **Specimen Description**: Type, anatomical source, laterality, size, and fixation method.
- **Anesthesia Type**: Classification of anesthesia approach (general, local, sedation).
- **Case Status**: Current lifecycle state (requested, scheduled, pre-operative-cleared, in-surgery, post-operative, closed).

### Invariants

- A surgical case cannot proceed to surgery without a completed pre-operative assessment.
- Informed consent must be documented and signed before the procedure begins.
- An operative note must be completed within 24 hours of procedure completion.
- Complications must be linked to a specific procedure and documented within the case.
- Pathology results must be reviewed and acknowledged by the operating surgeon.
- A case cannot be closed until all post-operative milestones are met.

## Domain Events Published

- **SurgicalCaseScheduled**: Notifies scheduling and pre-operative preparation workflows.
- **SurgeryCompleted**: Triggers post-operative workflows and notifies relevant subspecialty contexts (Hearing Services for cochlear implant cases, Voice Disorders for phonosurgery cases).
- **PathologyResultReceived**: Triggers pathology review and potentially oncologic staging workflows.
- **PostOperativeComplicationRecorded**: Triggers quality monitoring and complication management workflows.

## Domain Events Consumed

- **ImagingStudyReviewed** (from Clinical Assessment): Incorporates imaging findings into pre-operative surgical planning.
- **SleepSurgeryReferred** (from Sleep Disorders): Initiates pre-operative evaluation for sleep apnea surgery.
- **SinusitisSurgeryReferred** (from Allergy Management): Initiates pre-operative evaluation for sinus surgery.

## Domain Services

- **SurgicalEligibilityService**: Evaluates patient eligibility for specific procedures based on clinical criteria, imaging findings, and medical comorbidities.
- **SurgicalRiskAssessmentService**: Calculates composite surgical risk from patient factors and procedure characteristics.

## Procedure-Specific Modeling

Each procedure type carries domain-specific requirements:

- **Tonsillectomy**: Requires documentation of recurrence criteria (Paradise criteria) or obstruction severity. Post-operative plan emphasizes pain management and bleeding precautions.
- **Septoplasty**: Requires nasal obstruction documentation, failed medical therapy, and CT imaging. Operative note captures specific septal deviation characteristics.
- **FESS**: Requires CT staging (Lund-Mackay score), documentation of failed medical therapy, and anatomical variant identification. Operative note is structured by sinus compartment.
- **Thyroidectomy**: Requires fine needle aspiration cytology results (Bethesda classification), vocal fold mobility assessment, and thyroid function studies. Post-operative monitoring includes calcium and voice assessment.
- **Head and Neck Oncology**: Requires TNM staging, multidisciplinary tumor board discussion, and reconstructive planning. Pathology results drive adjuvant therapy decisions.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Flint, Paul W., et al. *Cummings Otolaryngology: Head and Neck Surgery*. 7th ed., Elsevier, 2020.
- American Academy of Otolaryngology-Head and Neck Surgery. Clinical Practice Guidelines on Tonsillectomy, Sinusitis, and Thyroid Nodules. https://www.entnet.org.
