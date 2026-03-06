# Entities: Attention-Deficit Domain

Entities are domain objects defined by their unique identity rather than their attributes. In the ADHD management domain, entities represent the core participants, processes, and records that persist over time and undergo state changes throughout the lifecycle of ADHD care.

## Purpose

ADHD management tracks individuals, assessments, treatments, and outcomes over extended periods, often spanning years from initial diagnosis through ongoing management. Entities provide the stable identity anchors that allow the system to track a patient's diagnostic history, treatment progression, behavioral changes, and educational accommodations across time, even as the attributes of these objects change continuously.

## Clinical Assessment Context Entities

### Patient

The Patient entity represents an individual undergoing ADHD evaluation and management. Identity persists across all assessment sessions, treatment episodes, and monitoring periods. Key attributes include demographic information, referral source, primary care provider, and insurance information. The patient's identity remains constant even as their diagnosis, treatment plan, and functional status evolve.

### Clinician

The Clinician entity represents a licensed professional conducting assessments or providing treatment. Identity encompasses credentials, specializations, and practice setting. A clinician's qualifications and scope of practice may change over time, but their identity within the system remains stable.

### Assessment Session

The Assessment Session entity represents a discrete evaluation encounter with a specific date, location, clinician, and set of administered instruments. Each session has a unique identity because the same instruments may be administered multiple times to the same patient with different results. Sessions accumulate over time to build the patient's comprehensive assessment history.

## Treatment Management Context Entities

### Treatment Episode

The Treatment Episode entity represents a bounded period of therapeutic intervention with a defined start, planned duration, and treatment goals. Episodes have unique identity because the same patient may undergo multiple episodes of the same treatment modality at different times with different goals and outcomes.

### Therapist

The Therapist entity represents a behavioral health provider delivering treatment interventions. Identity includes therapeutic orientation, certification status, and specialization areas. A therapist may work across multiple treatment episodes and with multiple patients.

### Coaching Engagement

The Coaching Engagement entity represents an ongoing relationship between a patient and an ADHD coach focused on executive function skill building, organizational strategies, and goal-directed behavior development. Each engagement has unique identity with its own set of coaching objectives and session history.

## Behavioral Monitoring Context Entities

### Behavior Observer

The Behavior Observer entity represents an individual (parent, teacher, clinician, self) who records behavioral observations. Observer identity is important because inter-rater reliability and observer bias must be tracked. The same behavior observed by different observers may yield different recordings.

### Monitoring Period

The Monitoring Period entity represents a defined timeframe during which behavioral data is systematically collected. Periods have unique identity because monitoring parameters (target behaviors, measurement methods, settings) may change between periods.

## Educational Accommodation Context Entities

### Educator

The Educator entity represents a teacher, special education specialist, or school administrator involved in accommodation planning and implementation. Identity persists across academic years and plan review cycles.

### School Placement

The School Placement entity represents a student's enrollment in a specific school, grade, and classroom configuration. Placement identity matters because accommodations are often tied to specific educational settings and may need modification when placement changes.

## Medication Management Context Entities

### Prescriber

The Prescriber entity represents the licensed clinician authorized to prescribe and manage ADHD medications. Identity includes prescribing authority, DEA registration (for controlled substances), and specialization.

### Medication Trial

The Medication Trial entity represents a discrete period during which a specific medication is prescribed at specific dosages. Trials have unique identity because the same medication may be tried at different times with different dosages and different outcomes.

## Outcomes Tracking Context Entities

### Research Cohort

The Research Cohort entity represents a defined group of patients whose outcomes are tracked collectively for comparative analysis. Cohort identity enables longitudinal outcome studies and population-level effectiveness evaluation.

## Entity Design Principles

**Identity over attributes.** Two Assessment Sessions on the same date with the same instruments are still distinct entities. The identity is what distinguishes them, not their attributes.

**Lifecycle tracking.** Entities in the ADHD domain often have complex lifecycles (e.g., a Treatment Episode moves through planned, active, paused, completed, and discontinued states). The entity's identity persists across all lifecycle states.

**Cross-context identity correlation.** The same real-world individual may be represented as a Patient entity in one context and a Student entity in another. Shared identifiers enable correlation without coupling the entity definitions.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
