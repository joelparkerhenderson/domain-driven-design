# Domain Events in the Psychiatry Domain

## Overview

A domain event represents a significant occurrence within the domain that domain experts care about. Domain events capture the fact that something happened at a specific point in time and carry the data needed for other parts of the system to react. In the psychiatry domain, domain events represent clinically significant occurrences such as a diagnosis being assigned, a medication being prescribed, a crisis being activated, or a treatment milestone being achieved. These events drive inter-context communication and maintain an auditable clinical timeline.

## Characteristics

### Immutability

Domain events are immutable records of something that happened. Once a Diagnosis Assigned event is created, it cannot be modified. If a diagnosis is later revised, a new Diagnosis Revised event is published. This immutability provides a reliable clinical audit trail.

### Temporal Significance

Every domain event has a timestamp indicating when the occurrence happened. In psychiatry, temporal ordering is clinically important: the sequence of diagnosis, medication initiation, symptom change, and outcome measurement tells a clinical story that informs treatment decisions.

### Eventual Consistency

Domain events enable eventual consistency between bounded contexts. When a diagnosis is assigned in the Diagnostic Assessment Context, the Medication Management Context does not learn about it instantaneously. The Diagnosis Assigned event propagates asynchronously, and the medication context updates its view of the patient's diagnostic profile upon receiving the event. This delay is clinically acceptable because prescribing decisions are not made in the same instant as diagnosis assignment.

## Domain Events by Bounded Context

### Diagnostic Assessment Context

**Evaluation Initiated**
- Payload: evaluation ID, patient ID, evaluating clinician ID, evaluation type, initiation timestamp
- Consumers: Outcomes Measurement Context (to schedule baseline measurements)

**Diagnosis Assigned**
- Payload: evaluation ID, patient ID, diagnosis code, diagnostic specifiers, assigning clinician ID, confidence level, assignment timestamp
- Consumers: Medication Management Context (to update indication profile), Psychotherapy Context (to recommend treatment protocols), Outcomes Measurement Context (to configure measurement batteries)

**Diagnosis Revised**
- Payload: evaluation ID, patient ID, previous diagnosis code, revised diagnosis code, revision reason, revising clinician ID, revision timestamp
- Consumers: Medication Management Context (to reassess medication indications), Psychotherapy Context (to reassess modality appropriateness)

**Rating Scale Completed**
- Payload: administration ID, patient ID, instrument name, total score, severity interpretation, administration timestamp
- Consumers: Outcomes Measurement Context (to incorporate into outcome tracking)

### Medication Management Context

**Medication Prescribed**
- Payload: prescription ID, patient ID, medication name, dosage, prescribing clinician ID, indication, prescription timestamp
- Consumers: Outcomes Measurement Context (to track treatment changes), Crisis Management Context (to update current medication list for emergency reference)

**Medication Discontinued**
- Payload: prescription ID, patient ID, medication name, discontinuation reason, discontinuing clinician ID, discontinuation timestamp
- Consumers: Outcomes Measurement Context, Crisis Management Context

**Adverse Event Reported**
- Payload: event ID, patient ID, medication reference, adverse event description, severity, reporter ID, report timestamp
- Consumers: Outcomes Measurement Context (to track safety outcomes)

**Drug Interaction Detected**
- Payload: interaction ID, patient ID, drug pair, interaction severity, clinical recommendation, detection timestamp
- Consumers: Prescribing clinician notification (within context)

### Psychotherapy Context

**Therapy Course Started**
- Payload: course ID, patient ID, therapist ID, modality, treatment protocol reference, start timestamp
- Consumers: Outcomes Measurement Context (to initiate therapy outcome tracking)

**Session Completed**
- Payload: session ID, course ID, patient ID, session date, session type, duration, interventions delivered, outcome rating
- Consumers: Outcomes Measurement Context (to track session-level outcomes)

**Therapy Course Terminated**
- Payload: course ID, patient ID, termination reason (completed, dropout, clinician-initiated, mutual), termination timestamp
- Consumers: Outcomes Measurement Context (to calculate end-of-treatment outcomes)

### Crisis Management Context

**Crisis Activated**
- Payload: crisis event ID, patient ID, activation reason, risk level, responding clinician ID, activation timestamp
- Consumers: Medication Management Context (to flag active crisis on medication record), Psychotherapy Context (to notify treating therapist)

**Involuntary Hold Initiated**
- Payload: hold ID, patient ID, hold type, initiating clinician ID, hold reason, initiation timestamp
- Consumers: Outcomes Measurement Context (to track crisis outcomes), external reporting (state mandated reporting)

**Crisis Resolved**
- Payload: crisis event ID, patient ID, resolution disposition, follow-up plan, resolution timestamp
- Consumers: Psychotherapy Context (to schedule follow-up), Outcomes Measurement Context

**Safety Plan Created**
- Payload: safety plan ID, patient ID, creating clinician ID, creation timestamp
- Consumers: Crisis Management Context (internal reference for future crises)

### Rehabilitation Services Context

**Rehabilitation Plan Created**
- Payload: plan ID, patient ID, service categories, creating case manager ID, creation timestamp
- Consumers: Outcomes Measurement Context (to track rehabilitation outcomes)

**Milestone Achieved**
- Payload: milestone ID, plan ID, patient ID, milestone description, achievement date, evidence summary
- Consumers: Outcomes Measurement Context (to track functional progress)

### Outcomes Measurement Context

**Outcome Report Generated**
- Payload: report ID, patient ID, reporting period, summary scores, clinically significant change classifications, generation timestamp
- Consumers: All clinical contexts (to inform treatment decisions with outcome data)

## Event Sourcing Considerations

The psychiatry domain benefits from event sourcing for the diagnostic and medication histories, where the complete sequence of changes is clinically significant. A patient's diagnostic history is best represented as a stream of Diagnosis Assigned, Diagnosis Revised, and Diagnosis Resolved events rather than a mutable current-state record. This preserves the clinical narrative that informs future treatment decisions.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on domain events and event-driven architecture.
