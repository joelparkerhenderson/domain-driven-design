# Aggregates: Autism Domain

Aggregates are clusters of related domain objects treated as a single unit for data consistency, defining transactional boundaries within each bounded context of the autism spectrum management domain.

## Purpose

In the autism domain, complex clinical and educational processes involve many interrelated objects that must maintain consistency. Aggregates group these objects around a root entity, ensuring that all invariants are enforced within the aggregate boundary. External objects reference the aggregate only through its root, preventing inconsistent state changes from bypassing business rules.

## Diagnostic Assessment Context Aggregates

### Diagnostic Evaluation Aggregate

Root entity: DiagnosticEvaluation. Contains the complete evaluation process for one individual, including screening results, instrument administrations, clinical observations, and the diagnostic determination. Invariants: a diagnostic evaluation must include at least one standardized instrument administration before a determination can be recorded. The evaluation status follows a defined lifecycle (Initiated, Screening Complete, Instruments Administered, Determination Made, Report Finalized).

### Assessment Instrument Aggregate

Root entity: InstrumentAdministration. Contains the administration of a single standardized assessment instrument (ADOS-2, ADI-R, Bayley-4) including item-level responses, domain scores, and composite scores. Invariants: all required items must be scored before domain scores can be computed. Scores must fall within valid ranges defined by the instrument's scoring manual.

## Behavioral Intervention Context Aggregates

### Intervention Plan Aggregate

Root entity: InterventionPlan. Contains the behavior intervention plan (BIP) including target behaviors, functional behavior assessment results, replacement behaviors, antecedent strategies, consequence strategies, and crisis protocols. Invariants: every target behavior must have at least one replacement behavior. The plan must reference a completed functional behavior assessment.

### Skill Acquisition Program Aggregate

Root entity: SkillAcquisitionProgram. Contains a structured teaching program including the target skill, teaching procedures (DTT or NET), prompt hierarchy, mastery criteria, and generalization plan. Invariants: mastery criteria must be defined before the program can be activated. The prompt hierarchy must specify a fading schedule.

### Therapy Session Aggregate

Root entity: TherapySession. Contains a single ABA therapy session including session metadata (date, duration, therapist), trial data, behavior incident records, and reinforcement data. Invariants: session duration must be within authorized service hours. All trial data entries must reference an active skill acquisition program.

## Sensory Processing Context Aggregates

### Sensory Profile Aggregate

Root entity: SensoryProfile. Contains the comprehensive sensory processing assessment for one individual, including responses across all sensory modalities (visual, auditory, tactile, vestibular, proprioceptive, olfactory, gustatory), quadrant scores (seeking, avoiding, sensitivity, registration), and clinical interpretation. Invariants: all sensory modalities must be assessed before the profile is considered complete.

### Sensory Diet Aggregate

Root entity: SensoryDiet. Contains a personalized plan of sensory activities organized by time of day, including activity descriptions, target sensory systems, duration, frequency, and caregiver instructions. Invariants: a sensory diet must reference a completed sensory profile. Activities must cover the sensory systems identified as areas of concern in the profile.

## Communication Support Context Aggregates

### Communication Plan Aggregate

Root entity: CommunicationPlan. Contains the comprehensive communication intervention plan including current communication abilities, communication goals, AAC recommendations, therapy approach, and family training components. Invariants: goals must be based on a documented communication assessment. AAC recommendations must include a device trial period.

### AAC Configuration Aggregate

Root entity: AACConfiguration. Contains the setup and customization of an AAC device or system for one individual, including vocabulary selection, display layout, access method, and voice output settings. Invariants: the access method must match the individual's motor capabilities as documented in the communication assessment. Vocabulary must include core words appropriate to the individual's communication level.

## Educational Planning Context Aggregates

### IEP Document Aggregate

Root entity: IEPDocument. Contains the complete Individualized Education Program including present levels of performance, annual goals, short-term objectives, related services, accommodations, modifications, and placement determination. Invariants: the IEP must include measurable annual goals. Related services must specify frequency, duration, and location. The IEP must be reviewed at least annually.

### Transition Plan Aggregate

Root entity: TransitionPlan. Contains post-secondary transition planning components including transition assessments, post-secondary goals (education, employment, independent living), transition services, and agency linkages. Invariants: transition planning must begin no later than the IEP in effect when the student turns 16. Post-secondary goals must be based on age-appropriate transition assessments.

## Outcomes Tracking Context Aggregates

### Outcome Measure Aggregate

Root entity: OutcomeMeasure. Contains a structured measurement of treatment effectiveness for one individual across defined outcome domains, including baseline data, periodic measurements, and trend analysis. Invariants: outcome measures must reference specific intervention goals. Trend calculations require at least three data points.

### Progress Report Aggregate

Root entity: ProgressReport. Contains a summary of progress across all active interventions for a defined reporting period, including data from behavioral, sensory, communication, and educational contexts. Invariants: a progress report must cover at least two consecutive measurement periods. Data sources must be identified for each reported metric.

## Aggregate Design Principles

- Keep aggregates small: Each aggregate should contain only the objects necessary to enforce its invariants.
- Reference other aggregates by identity: Aggregates reference each other through root entity identifiers, not direct object references.
- One transaction per aggregate: Each business operation should modify only one aggregate to maintain consistency without distributed transactions.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 6 on aggregates.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 10 on aggregate design rules.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8 on aggregate boundaries and consistency.
