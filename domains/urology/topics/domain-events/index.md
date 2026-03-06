# Domain Events in the Urology Domain

## Overview

A domain event represents a significant occurrence within the domain that domain experts care about. Domain events capture something that happened in the past and are named using past-tense verbs. In the urology domain, domain events enable loose coupling between bounded contexts by allowing one context to react to changes in another without direct dependencies. Events are the primary mechanism for cross-context communication in the urology system.

## Domain Event Characteristics

Domain events in the urology domain are immutable records of past occurrences. Each event includes a unique identifier, a timestamp, the identity of the aggregate that produced it, and a payload containing the event-specific data. Events are published by aggregate roots after a state change and consumed by event handlers in other contexts or within the same context.

## Clinical Assessment Events

### DiagnosticWorkupCompleted

Published when a urological diagnostic workup reaches a definitive conclusion. The event payload includes the patient identifier, workup identifier, diagnostic conclusion, severity classification, and recommended next steps. This event is consumed by treatment contexts (Oncologic, Stone Disease, Incontinence, Male Reproductive Health) to initiate treatment planning based on the diagnosis.

### UrodynamicStudyRecorded

Published when urodynamic testing is completed and interpreted. The payload includes bladder capacity, compliance, presence of detrusor overactivity, maximum flow rate, post-void residual, and the interpreting physician's conclusion. Consumed by the Incontinence Management Context and the Surgical Management Context to inform treatment selection.

### ImagingSuspiciousLesionIdentified

Published when imaging reveals a finding requiring further evaluation, such as a PI-RADS 4-5 prostate lesion, an enhancing renal mass, or a filling defect in the bladder. The event triggers downstream actions in the Oncologic Urology Context for cancer workup or in the Stone Disease Context for stone characterization.

## Surgical Management Events

### SurgeryCompleted

Published when a surgical procedure finishes. The payload includes the case identifier, procedure type, operative duration, estimated blood loss, complications (classified by Clavien-Dindo grade), specimen details, and immediate post-operative status. Consumed by the Oncologic Context (for pathological staging of surgical specimens), the Stone Disease Context (for stone-free status assessment), and the Clinical Assessment Context (for updating the patient's diagnostic profile).

### SurgicalComplicationRecorded

Published when a post-operative complication is identified. The payload includes the case identifier, complication type, Clavien-Dindo grade, date of recognition, and intervention required. Consumed by quality assurance systems and the Clinical Assessment Context for updating the patient's clinical status.

### RoboticSystemEventLogged

Published when a significant robotic system event occurs during surgery, such as instrument malfunction, system error, or conversion to open approach. The payload includes the event type, timestamp relative to procedure start, impact on the procedure, and resolution.

## Oncologic Urology Events

### BiopsyResultReceived

Published when pathology results for a urological biopsy become available. The payload includes the biopsy type (prostate systematic, MRI-fusion targeted, bladder, renal), individual core results, aggregate statistics, and pathological diagnosis. This event triggers staging updates and treatment planning in the Oncologic Context.

### CancerStageUpdated

Published when a patient's cancer staging changes due to new information (post-surgical pathology, imaging restaging, biochemical recurrence). The payload includes the previous stage, new stage, reason for change, and clinical implications. Consumed by the Surveillance Plan aggregate and treatment decision workflows.

### PSAThresholdBreached

Published when a PSA measurement crosses a clinically significant threshold (detectable PSA after prostatectomy, rising PSA above a surveillance trigger, PSA velocity exceeding 0.75 ng/mL/year). The event triggers clinical review and potential treatment plan modification.

## Stone Disease Events

### StoneAnalysisReturned

Published when stone composition analysis results are available from the laboratory. The payload includes the stone identifier, mineral composition percentages, and clinical interpretation. Consumed by the MetabolicProfile aggregate to guide targeted prevention strategies based on stone type.

### MetabolicAbnormalityIdentified

Published when a 24-hour urine metabolic workup reveals a significant abnormality (hypercalciuria, hyperoxaluria, hypocitraturia). The payload includes the specific abnormality, measured value, reference range, and recommended intervention. Triggers dietary counseling and pharmacologic prevention planning.

### StoneRecurrenceDetected

Published when imaging reveals new stone formation in a patient with a history of nephrolithiasis. The event triggers reassessment of the metabolic prevention plan and consideration of intervention.

## Incontinence Management Events

### ContinenceStatusChanged

Published when a patient's continence status changes, either improvement (achieving dryness after intervention) or worsening (recurrence of incontinence). The payload includes the previous and current continence classification, the measurement method (pad weight, patient-reported), and the triggering event.

### PelvicFloorTherapyMilestoneReached

Published when a patient achieves a defined milestone in pelvic floor rehabilitation, such as achieving target contraction strength or reaching a predefined pad-free interval. Consumed by the treatment planning workflow to assess whether conservative management is sufficient or surgical intervention is indicated.

## Male Reproductive Health Events

### SemenAnalysisCompleted

Published when a semen analysis is finalized. The payload includes all WHO-reference parameters and their interpretations. Consumed by the FertilityEvaluation aggregate to assess whether further workup (hormonal testing, genetic testing, imaging) is indicated based on the results.

### TestosteroneDeficiencyConfirmed

Published when repeat morning testosterone measurements confirm hypogonadism. The payload includes the testosterone values, confirmatory pattern, and classification (primary versus secondary). Triggers treatment planning for testosterone replacement therapy and the associated monitoring protocol.

## Event Ordering and Idempotency

Domain events in the urology domain must be processed in causal order within an aggregate but may be processed in any order across aggregates. Event handlers must be idempotent because events may be delivered more than once in distributed systems. Each handler checks whether it has already processed an event before applying side effects.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6.
- Brandolini, Alberto. Introducing EventStorming. Leanpub, 2021.
