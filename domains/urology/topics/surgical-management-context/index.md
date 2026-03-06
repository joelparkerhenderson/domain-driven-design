# Surgical Management Context

## Overview

The Surgical Management Context encompasses all procedural interventions in the urology domain, from minimally invasive endoscopic procedures to complex robotic-assisted operations. This bounded context manages the lifecycle of a surgical case from pre-operative planning through intraoperative execution to post-operative recovery and complication tracking. It serves as a shared procedural capability consumed by the Oncologic, Stone Disease, Incontinence, and Male Reproductive Health contexts.

## Scope

This context covers robotic-assisted surgery (prostatectomy, partial nephrectomy, cystectomy, pyeloplasty), laparoscopic procedures, open surgical approaches, endourology (ureteroscopy, cystoscopy with intervention, transurethral resection), percutaneous procedures (PCNL, nephrostomy), and reconstructive urology (urethroplasty, augmentation cystoplasty, urinary diversion). It also manages surgical instrumentation, operating room resource coordination, and surgical outcome tracking.

## Ubiquitous Language

Within this context, "case" refers to a complete surgical episode. "Approach" specifies the surgical technique (robotic, laparoscopic, open, endoscopic, percutaneous). "Console time" measures the duration of active robotic manipulation. "Conversion" indicates a change from planned minimally invasive to open approach. "Margin status" describes whether the surgical specimen edge is free of disease. "Clavien-Dindo grade" classifies post-operative complications from I (minor deviation) to V (death).

## Aggregates

The SurgicalCase aggregate root manages the complete procedural episode. It contains PreoperativeAssessment, OperativePlan, IntraoperativeRecord, and PostoperativeRecovery entities. The RoboticProcedure aggregate extends SurgicalCase with robotic-specific data including console time, port configuration, and instrument utilization. Each aggregate enforces invariants such as requiring completed pre-operative clearance before scheduling.

## Key Entities

The SurgicalCase entity tracks case state from scheduled through completed. IntraoperativeEvent entities record significant occurrences during surgery. SurgicalSpecimen entities track tissue removed for pathological examination. PostoperativeComplication entities record adverse events with timing, severity, and management.

## Value Objects

OperativeDuration captures total operative time, console time, and docking time for robotic cases. EstimatedBloodLoss records intraoperative blood loss in milliliters. ClavienDindoGrade classifies complication severity. PortPlacement records trocar positions for minimally invasive procedures. MarginStatus indicates positive, negative, or indeterminate surgical margins with location specificity.

## Domain Events

SurgeryCompleted is the primary event published by this context, consumed by the originating treatment context (Oncologic, Stone Disease, Incontinence, or Male Reproductive Health) to update treatment status and initiate post-operative protocols. SurgicalComplicationRecorded triggers quality review processes and updates the patient's clinical status. SpecimenSubmitted notifies the pathology system that tissue requires processing.

## Surgical Planning Workflows

Pre-operative planning integrates diagnostic data from the Clinical Assessment Context. For oncologic cases, the Oncologic Context provides cancer staging that determines the procedure type and extent. For stone cases, the Stone Disease Context provides stone characteristics that determine the procedural approach. The Surgical Management Context translates these clinical requirements into operative plans with specific technical details.

## Quality and Outcomes

This context maintains comprehensive surgical outcome data. It tracks operative metrics (duration, blood loss, conversion rates), functional outcomes (continence rates after prostatectomy, stone-free rates after lithotripsy), oncologic outcomes (positive margin rates, recurrence rates), and complication rates by procedure type, surgeon, and approach. These metrics support continuous quality improvement and informed consent processes.

## Integration Points

The Surgical Management Context integrates with the Clinical Assessment Context for pre-operative diagnostic data. It sends surgical outcomes to the originating treatment context (Oncologic, Stone Disease, Incontinence, Male Reproductive Health). It interfaces with pathology systems for specimen tracking. It communicates with anesthesia systems for anesthetic records and with nursing systems for perioperative care documentation.

## Business Rules

A surgical case cannot proceed without documented informed consent and completed pre-operative assessment. The operative plan must be consistent with the indication provided by the referring treatment context. Surgical complications must be documented within 30 days of the procedure. Specimen handling must follow chain-of-custody protocols from operative field to pathology laboratory. Robotic procedures must document mandatory safety checks at each phase (docking, undocking, specimen extraction).

## Relationship to Other Contexts

The Surgical Management Context is a supplier to multiple customer contexts. The Oncologic, Stone Disease, Incontinence, and Male Reproductive Health contexts request surgical services and receive operative outcomes. This context does not make clinical decisions about whether surgery is indicated; it receives the decision from the appropriate treatment context and manages the procedural execution.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Dindo, D., Demartines, N., and Clavien, P.A. "Classification of Surgical Complications." Annals of Surgery, 2004.
- Intuitive Surgical. da Vinci Surgical System Clinical Evidence. https://www.intuitive.com
