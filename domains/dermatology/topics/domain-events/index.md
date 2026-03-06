# Domain Events in the Dermatology Domain

Domain events are records of significant occurrences within the domain that other parts of the system may need to react to. In the dermatology domain, domain events capture meaningful clinical happenings such as a suspicious lesion being identified, a biopsy being performed, a pathology report being completed, or a treatment outcome being assessed. As Evans (2003) describes, domain events make implicit domain concepts explicit, enabling loosely coupled communication between bounded contexts.

## Characteristics of Dermatology Domain Events

Domain events in the dermatology domain are immutable records of something that has already happened. Each event carries a unique event identifier, a timestamp, the identity of the originating aggregate, and the payload of domain-relevant data. Events are named in past tense to reflect that they describe completed occurrences. They are published by the aggregate that experienced the change and consumed by any interested bounded context.

## Clinical Assessment Context Events

### LesionIdentified

Published when a dermatologist documents a new lesion finding during a skin examination. The event carries the examination identifier, lesion finding identifier, anatomical location, morphological descriptors, dermoscopic pattern, initial risk assessment, and examining provider. The Lesion Management Context consumes this event to determine whether a new Lesion Record should be created for tracking.

### SkinExaminationCompleted

Published when a skin examination is finalized with all findings documented. The event carries the examination identifier, patient identifier, examination date, total number of findings, and overall clinical impression. The Treatment Outcomes Context may consume this event to update baseline assessments for ongoing treatment evaluations.

### SuspiciousLesionFlagged

Published when a lesion finding meets criteria for heightened concern (irregular dermoscopic pattern, ABCDE score above threshold, or significant evolution since last examination). The event carries the examination and lesion identifiers, the specific criteria that triggered the flag, and the recommended urgency level. The Lesion Management Context consumes this to prioritize biopsy evaluation.

## Lesion Management Context Events

### BiopsyDecisionMade

Published when a clinical decision to biopsy a specific lesion is documented. The event carries the lesion record identifier, biopsy type selected, clinical indication, and target date. The Procedure Management Context may consume this to schedule the biopsy procedure, and the Pathology Integration Context prepares for specimen receipt.

### BiopsyPerformed

Published when a biopsy is physically collected from a lesion. The event carries the lesion identifier, biopsy type performed, specimen details, collecting provider, collection date, and the clinical context summary to accompany the specimen. The Pathology Integration Context consumes this to initiate specimen accessioning and tracking.

### LesionDiagnosisConfirmed

Published when a definitive diagnosis is established for a lesion, typically following pathology report review. The event carries the lesion identifier, confirmed diagnosis (ICD-10 code and description), diagnosis method, and any staging information. The Procedure Management Context consumes this to determine appropriate treatment protocols, and the Treatment Outcomes Context initiates outcome tracking.

### ExcisionPlanFinalized

Published when a surgical excision plan is approved with specific margin requirements and technique selection. The event carries the lesion identifier, planned margins, excision technique, and reconstruction approach. The Procedure Management Context consumes this to schedule and prepare for the excision.

### RecurrenceDetected

Published when a previously treated lesion shows signs of recurrence during follow-up. The event carries the lesion identifier, recurrence characteristics, time since last treatment, and previous treatment details. The Procedure Management Context and Treatment Outcomes Context both consume this event.

## Procedure Management Context Events

### ProcedureScheduled

Published when a dermatologic procedure is scheduled. The event carries the procedure session identifier, procedure type, patient identifier, scheduled date, and any pre-procedure requirements. Relevant contexts consume this for coordination purposes.

### ProcedureCompleted

Published when a procedure is finished and documented. The event carries the procedure session identifier, procedure type, completion status, key outcome parameters (margins achieved, number of Mohs stages, treatment area details), and any complications noted. The Treatment Outcomes Context consumes this to initiate post-procedure outcome tracking.

### MohsStageCompleted

Published after each stage of Mohs micrographic surgery is examined. The event carries the procedure session identifier, stage number, margin status for each section, and whether additional stages are required. This event is primarily consumed within the Procedure Management Context for surgical workflow coordination.

## Cosmetic Services Context Events

### CosmeticConsentObtained

Published when a patient signs informed consent for a cosmetic procedure. The event carries the consent identifier, patient identifier, procedure type, consent date, and expiration date. The Cosmetic Services Context uses this as a prerequisite for any treatment administration.

### InjectableTreatmentAdministered

Published when an injectable treatment (neuromodulator or filler) is administered. The event carries the treatment plan identifier, product type and lot number, units or volume administered, treatment areas, and administering provider. The Treatment Outcomes Context may consume this to track cosmetic treatment outcomes.

## Pathology Integration Context Events

### SpecimenAccessioned

Published when a biopsy specimen is formally registered in the pathology laboratory. The event carries the specimen accession number, collection details, and estimated processing timeline. The Lesion Management Context consumes this for specimen tracking status updates.

### PathologyReportCompleted

Published when the pathologist finalizes the histopathology report for a specimen. The event carries the specimen accession number, histopathological diagnosis, margin status, staging information (if applicable), and any additional recommendations. The Lesion Management Context consumes this to update the lesion diagnosis, and the Treatment Outcomes Context uses it for outcome correlation.

## Treatment Outcomes Context Events

### SeverityAssessmentRecorded

Published when a standardized severity assessment (PASI, SCORAD, DLQI) is completed. The event carries the treatment outcome record identifier, scoring system used, score value, assessment date, and change from previous assessment. The Clinical Assessment Context may consume this for clinical decision support during subsequent encounters.

### TreatmentResponseClassified

Published when a treatment response is categorized (complete response, partial response, stable, or progressed). The event carries the treatment outcome identifier, response category, supporting measurements, and assessment date.

## Event Design Principles

Following Vernon's (2013) guidelines, domain events in the dermatology domain carry sufficient data for consumers to act without needing to query back to the originating context. Events are designed to be self-contained while avoiding unnecessary data duplication. Khononov (2021) emphasizes that events should represent business-meaningful occurrences rather than technical state changes.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9 on domain events and integration.
