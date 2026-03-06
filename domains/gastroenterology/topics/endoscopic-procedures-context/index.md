# Endoscopic Procedures Context

## Overview

The Endoscopic Procedures Context manages the complete lifecycle of endoscopic
procedures performed in gastroenterology practice. This includes esophagogastro-
duodenoscopy (EGD), colonoscopy, endoscopic retrograde cholangiopancreatography
(ERCP), endoscopic ultrasound (EUS), and capsule endoscopy. The context spans
procedure scheduling, informed consent, sedation management, procedural documentation,
therapeutic interventions, specimen collection, and pathology result tracking.

## Scope and Responsibilities

This context is responsible for:

- Managing procedure scheduling with preparation requirements.
- Documenting informed consent including risks, benefits, and alternatives.
- Recording sedation type, medications, and monitoring during procedures.
- Capturing procedural findings with standardized terminology and grading.
- Documenting therapeutic interventions performed during procedures.
- Managing specimen collection, labeling, and pathology submission.
- Tracking pathology results and linking them to procedure findings.
- Calculating and recording quality metrics for each procedure.
- Determining surveillance intervals based on findings and guidelines.

## Core Aggregates

### EndoscopicProcedure

The primary aggregate of this context. Manages the full procedure lifecycle from
scheduling through final documentation. Contains the procedure type, indication,
patient preparation assessment, sedation record, endoscopic findings, therapeutic
interventions, complications, and quality metrics.

Key invariants: a completed procedure must have documented informed consent, a
sedation record, at least one documented finding (even if normal), and calculated
quality metrics appropriate to the procedure type. For colonoscopy, withdrawal time
must be recorded and cecal intubation documented.

### SpecimenRecord

Manages the lifecycle of tissue specimens and fluid collections obtained during
endoscopy. Each specimen has a unique accession number and tracks collection site,
specimen type, fixative, container, submission to pathology, and result receipt.

Key invariant: every collected specimen must have a documented anatomical collection
site and must be traceable through the pathology workflow. No specimen may exist
without an associated procedure.

## Key Entities

- ProceduralFinding: a specific observation made during endoscopy, with location,
  morphology, size, and photographic documentation reference.
- TherapeuticIntervention: an action performed during the procedure such as polypectomy,
  dilation, hemostasis, or stent placement.
- SedationRecord: medications administered, doses, vitals monitoring, and recovery
  assessment.

## Key Value Objects

- ProcedureType: enumeration of EGD, colonoscopy, ERCP, EUS, capsule endoscopy.
- BostonBowelPrepScore: bowel preparation quality scoring (0-9 scale across three
  segments).
- WithdrawalTime: measured duration of colonoscope withdrawal in minutes.
- PolypMorphology: Paris classification of polyp shape and size.
- PathologyResult: histological diagnosis from submitted specimens.
- CPTCode: procedure coding for documentation and billing.
- QualityMetric: procedure-specific quality measurement with benchmark comparison.

## Domain Events

- ProcedureScheduled: triggers preparation instruction delivery and resource allocation.
- ProcedureCompleted: notifies downstream contexts of findings and specimens collected.
- PathologyResultReceived: distributes histological results to disease management contexts.
- ComplicationOccurred: triggers adverse event documentation and clinical response.

## Clinical Workflows

### Pre-Procedure Workflow

When a procedure is scheduled, the context manages preparation requirements (bowel
prep for colonoscopy, fasting for EGD, anticoagulation management for all procedures),
informed consent documentation, and pre-procedure risk assessment. The ProcedureScheduled
event initiates these preparations.

### Intra-Procedure Workflow

During the procedure, findings are documented in real time with standardized terminology.
Therapeutic interventions are recorded with technique details. Specimens are collected,
labeled, and submitted. Sedation is monitored and documented continuously.

### Post-Procedure Workflow

After the procedure, quality metrics are calculated, the procedure report is generated,
and the ProcedureCompleted event is raised. Specimens enter pathology tracking.
Surveillance intervals are calculated based on findings and applicable guidelines.
The patient receives recovery assessment and discharge instructions.

### Pathology Integration Workflow

Pathology results arrive asynchronously after the procedure. The PathologyResultReceived
event distributes results to the Inflammatory Disease Context (for IBD surveillance
biopsies), the Clinical Assessment Context (for diagnostic biopsies), and back to the
procedure record for complete documentation.

## Quality Metrics

This context tracks key quality indicators for endoscopic practice:
- Adenoma detection rate for screening colonoscopy.
- Cecal intubation rate.
- Adequate bowel preparation rate.
- Withdrawal time compliance.
- Appropriate surveillance interval recommendation rate.
- Complication rates by procedure type.

## Integration Points

- Upstream: receives referrals from Clinical Assessment Context.
- Downstream: provides findings to Inflammatory Disease and Hepatology Contexts.
- External: sends specimens to pathology systems, receives pathology results.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapters 5-6.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapters 5, 10.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8.
- Rex, D.K. et al. (2017). "Quality indicators for colonoscopy." American Journal of Gastroenterology.
- Multi-society guidelines on post-polypectomy and post-cancer resection surveillance.
