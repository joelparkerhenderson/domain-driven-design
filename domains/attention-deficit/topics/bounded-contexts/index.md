# Bounded Contexts: Attention-Deficit Domain

Bounded contexts define the logical boundaries within the ADHD management domain where specific models, terminology, and rules remain internally consistent. Each bounded context encapsulates a distinct area of ADHD care with its own ubiquitous language, domain logic, and data ownership.

## Purpose

ADHD management involves multiple professional disciplines, each with its own vocabulary, workflows, and conceptual models. A clinician conducting a diagnostic evaluation thinks about DSM-5 criteria and rating scale scores. An educator developing a 504 plan thinks about classroom accommodations and academic modifications. A prescriber managing medication thinks about titration schedules and side effect profiles. Bounded contexts ensure that these distinct perspectives are modeled independently, preventing conceptual confusion and tight coupling.

## The Six Bounded Contexts

### Clinical Assessment Context

This context owns the diagnostic evaluation process. It manages referral intake, assessment session scheduling, administration of standardized rating scales (Conners, SNAP-IV, Vanderbilt), continuous performance testing, neuropsychological battery administration, clinical interviews, and the formulation of diagnostic impressions based on DSM-5 criteria. The aggregate root is the Patient Assessment, which encapsulates the complete diagnostic history for an individual.

### Treatment Management Context

This context coordinates multimodal treatment planning. It manages treatment episodes, behavioral therapy plans, parent training programs, coaching engagements, psychoeducation sessions, and environmental modification recommendations. The aggregate root is the Treatment Plan, which maintains consistency across all active interventions for a given patient.

### Behavioral Monitoring Context

This context captures longitudinal behavioral data. It manages daily symptom logs, behavior frequency charts, executive function performance tracking, attention span measurement, hyperactivity and impulsivity incident recording, and cross-setting behavioral comparisons. The aggregate root is the Behavior Record, which aggregates all monitoring data for a specific individual over time.

### Educational Accommodation Context

This context manages school-based supports. It handles IEP development and review, 504 plan creation and modification, classroom accommodation recommendations, academic support service coordination, teacher consultation records, and school-based intervention tracking. The aggregate root is the Accommodation Plan, which maintains the current set of educational supports for a student.

### Medication Management Context

This context handles pharmacological treatment. It manages stimulant and non-stimulant medication selection, dosage titration protocols, medication trial documentation, side effect monitoring, medication adherence tracking, and prescriber communication. The aggregate root is the Medication Regimen, which represents the current and historical pharmacological treatment for a patient.

### Outcomes Tracking Context

This context measures treatment effectiveness. It manages treatment response assessments, functional impairment ratings, quality of life measures, goal attainment scaling, longitudinal outcome analysis, and comparative effectiveness evaluation. The aggregate root is the Outcome Measure, which aggregates effectiveness data across all treatment modalities.

## Boundary Enforcement

Each bounded context enforces its boundaries through:

- **Separate domain models**: The same real-world individual is represented differently in each context (patient, student, client) with context-appropriate attributes.
- **Explicit interfaces**: Contexts communicate through well-defined integration points, not shared databases or internal object references.
- **Independent evolution**: Changes to the Clinical Assessment Context's diagnostic model do not require changes to the Educational Accommodation Context's accommodation model.
- **Team alignment**: Each context aligns with the expertise of a specific care team (clinicians, therapists, educators, prescribers, researchers).

## Cross-Context Identity

A shared patient identifier enables correlation across contexts without coupling their internal models. Each context maintains its own representation of the individual, translated through anti-corruption layers when data crosses boundaries.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- American Academy of Pediatrics. (2019). *Clinical Practice Guideline for the Diagnosis, Evaluation, and Treatment of ADHD in Children and Adolescents*.
