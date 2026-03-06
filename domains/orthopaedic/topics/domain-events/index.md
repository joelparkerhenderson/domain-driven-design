# Domain Events in the Orthopaedic Domain

## Overview

A domain event is a record of something significant that occurred within the domain.
Domain events are named in past tense using the ubiquitous language and carry the data
necessary for other parts of the system to react. In the orthopaedic domain, domain
events coordinate clinical workflows across bounded contexts without creating tight
coupling between them.

## Purpose

Orthopaedic care follows a pathway: assessment leads to diagnosis, diagnosis leads to
surgical planning or conservative treatment, surgery triggers rehabilitation. Domain
events model these transitions explicitly, allowing each bounded context to react to
upstream events autonomously. This ensures that when a surgeon completes an arthroplasty,
the Rehabilitation Context can automatically initiate the appropriate recovery protocol
without the Joint Replacement Context needing to know about rehabilitation details.

## Key Domain Events

### Clinical Assessment Events

**PatientAssessed**: Published when a clinical encounter is completed. Contains patient
identifier, examination findings summary, imaging study references, and provisional
diagnoses. Consumed by Surgical Planning, Fracture Management, and Sports Medicine
to initiate treatment pathways.

**ImagingStudyCompleted**: Published when imaging results are available. Contains
accession number, modality, anatomical region, and radiologist findings. Consumed
by Clinical Assessment to update encounter data.

**DiagnosisConfirmed**: Published when a provisional diagnosis is confirmed. Contains
patient identifier, diagnosis code, classification, and severity. Triggers downstream
treatment planning.

### Surgical Planning Events

**SurgicalPlanApproved**: Published when an operative plan is finalized. Contains plan
identifier, procedure type, selected implants, planned approach, and scheduled date.
Consumed by Joint Replacement and Fracture Management to prepare for surgery.

**ImplantSelectionFinalized**: Published when implant choices are locked. Contains
implant catalog numbers, sizes, and quantities. Used for inventory and supply chain
coordination.

### Joint Replacement Events

**ArthroplastyCompleted**: Published when a joint replacement surgery is finished.
Contains case identifier, patient identifier, implant component serial numbers,
bearing surface, cement usage, and intraoperative findings. Consumed by Rehabilitation
to initiate post-arthroplasty protocols.

**ImplantRegistered**: Published when implant data is recorded in the registry.
Contains implant tracking data for regulatory compliance.

**RevisionIndicated**: Published when clinical or radiographic findings suggest a
joint replacement may need revision. Triggers re-evaluation in Surgical Planning.

### Sports Medicine Events

**SportsInjuryDiagnosed**: Published when an athletic injury is classified. Contains
injury type, mechanism, severity, and sport. Consumed by Rehabilitation if
conservative or post-operative therapy is needed.

**ArthroscopyCompleted**: Published when an arthroscopic procedure is finished.
Contains procedure details, intraoperative findings, and post-operative restrictions.
Consumed by Rehabilitation.

**ReturnToPlayCleared**: Published when an athlete meets all return criteria. Contains
clearance date, functional test results, and any ongoing restrictions.

### Fracture Management Events

**FractureClassified**: Published when a fracture receives its AO/OTA classification.
Contains the classification code, displacement details, and associated injuries.
Consumed by Surgical Planning if operative treatment is indicated.

**FractureFixated**: Published when surgical fixation is completed. Contains fixation
method, hardware details, and reduction quality. Consumed by Rehabilitation.

**FractureUnionConfirmed**: Published when radiographic union is achieved. Contains
union date and final alignment. Triggers weight-bearing advancement in Rehabilitation.

### Rehabilitation Events

**RehabilitationEpisodeStarted**: Published when a rehabilitation program begins.
Contains episode identifier, protocol type, and initial assessment results.

**MilestoneAchieved**: Published when a patient reaches a functional milestone.
Contains milestone type, measurement values, and date achieved.

**RehabilitationCompleted**: Published when discharge criteria are met. Contains
final outcome scores and functional status.

## Event Design Principles

- Name events in past tense: something has already happened.
- Include all data consumers need; avoid requiring callbacks to the source context.
- Events are immutable once published.
- Include a timestamp and the identity of the aggregate that produced the event.
- Use a published language (schema) that is versioned and documented.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11.
