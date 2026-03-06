# Aggregates: Attention-Deficit Domain

Aggregates are clusters of related domain objects treated as a single unit for data consistency and transactional integrity. In the ADHD management domain, aggregates define the consistency boundaries around clinical assessments, treatment plans, behavior records, accommodation plans, medication regimens, and outcome measures.

## Purpose

ADHD management involves complex interdependencies between assessments, treatments, monitoring data, accommodations, medications, and outcomes. Aggregates enforce invariants within these clusters, ensuring that business rules are maintained even as the system evolves. Each aggregate has a root entity through which all external access occurs, protecting the internal consistency of the cluster.

## Primary Aggregates

### Patient Assessment Aggregate

The Patient Assessment aggregate is the root of the Clinical Assessment Context. It encapsulates:

- **Root Entity**: Patient Assessment, identified by a unique assessment identifier.
- **Child Entities**: Assessment Sessions, each representing a discrete evaluation encounter.
- **Value Objects**: DSM-5 Diagnostic Code, ADHD Presentation Type, Symptom Severity Level, Rating Scale Score, Comorbidity Profile.
- **Invariants**: A completed assessment must include at least two informant sources. Diagnostic impression must reference specific DSM-5 criteria met. Rating scale scores must fall within validated score ranges.

### Treatment Plan Aggregate

The Treatment Plan aggregate is the root of the Treatment Management Context. It encapsulates:

- **Root Entity**: Treatment Plan, identified by a unique plan identifier linked to a patient.
- **Child Entities**: Treatment Episodes, Intervention Assignments, Therapy Sessions.
- **Value Objects**: Treatment Modality, Session Frequency, Treatment Goal, Intervention Type.
- **Invariants**: A multimodal treatment plan must include at least two distinct modalities. Each intervention must have measurable behavioral objectives. Treatment goals must be time-bound with review dates.

### Behavior Record Aggregate

The Behavior Record aggregate is the root of the Behavioral Monitoring Context. It encapsulates:

- **Root Entity**: Behavior Record, identified by a unique record identifier for a specific individual.
- **Child Entities**: Daily Behavior Entries, Behavior Incident Reports, Executive Function Assessments.
- **Value Objects**: Symptom Frequency Count, Behavior Duration, Attention Span Measurement, Setting Type.
- **Invariants**: Behavior entries must include setting context (home, school, clinical). Frequency counts must be non-negative. Time-based measurements must have valid start and end timestamps.

### Accommodation Plan Aggregate

The Accommodation Plan aggregate is the root of the Educational Accommodation Context. It encapsulates:

- **Root Entity**: Accommodation Plan (IEP or 504), identified by a unique plan identifier.
- **Child Entities**: Accommodation Items, Service Assignments, Review Records.
- **Value Objects**: Accommodation Type, Service Duration, Eligibility Category, Review Cycle Period.
- **Invariants**: An active accommodation plan must have a current eligibility determination. Each accommodation item must specify implementation responsibility. Plans must have scheduled review dates within regulatory timelines.

### Medication Regimen Aggregate

The Medication Regimen aggregate is the root of the Medication Management Context. It encapsulates:

- **Root Entity**: Medication Regimen, identified by a unique regimen identifier.
- **Child Entities**: Medication Trials, Dosage Adjustments, Side Effect Reports.
- **Value Objects**: Medication Name, Dosage Amount, Administration Schedule, Side Effect Severity.
- **Invariants**: Active medications must not have known contraindications with each other. Dosage adjustments must fall within approved therapeutic ranges. Titration steps must follow prescribed intervals.

### Outcome Measure Aggregate

The Outcome Measure aggregate is the root of the Outcomes Tracking Context. It encapsulates:

- **Root Entity**: Outcome Measure, identified by a unique measure identifier.
- **Child Entities**: Measurement Points, Comparative Assessments, Goal Attainment Records.
- **Value Objects**: Functional Impairment Rating, Quality of Life Score, Treatment Response Category, Measurement Timepoint.
- **Invariants**: Outcome measurements must use validated instruments. Pre-treatment baseline must be established before post-treatment comparison. Measurement intervals must be consistent for longitudinal analysis.

## Aggregate Design Principles

**Keep aggregates small.** Each aggregate should enforce only the invariants that must be immediately consistent. Cross-aggregate consistency is handled through domain events and eventual consistency.

**Reference other aggregates by identity.** The Treatment Plan aggregate references a patient by identifier, not by holding a direct reference to the Patient Assessment aggregate. This maintains loose coupling between bounded contexts.

**Design for the most common operations.** Aggregate boundaries should optimize for the most frequent transactional operations in ADHD management workflows, such as recording a daily behavior entry or adjusting a medication dosage.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
