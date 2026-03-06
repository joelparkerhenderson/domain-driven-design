# Behavioral Monitoring Context: Attention-Deficit Domain

The Behavioral Monitoring Context manages the systematic tracking of ADHD symptoms, behavioral patterns, executive function performance, and daily functioning across settings and time periods. This context provides the longitudinal behavioral data that informs treatment decisions, medication adjustments, and outcomes evaluation.

## Context Purpose

ADHD symptoms fluctuate across settings (home, school, work, social), time of day (morning, afternoon, evening), activities (structured vs. unstructured), and treatment conditions (on medication vs. off medication). Effective ADHD management requires continuous behavioral data collection that captures these variations. This bounded context owns the infrastructure for systematic symptom tracking, behavior charting, and executive function monitoring, transforming raw behavioral observations into clinically actionable information.

## Aggregate Root: Behavior Record

The Behavior Record aggregate encapsulates all monitoring data for a specific individual over time. It maintains consistency across daily behavior entries, incident reports, executive function assessments, and cross-setting behavioral comparisons.

Key invariants enforced by this aggregate:

- Each behavior entry must specify the observation setting (home, school, clinical, community).
- Frequency counts must be non-negative integers with defined observation period duration.
- Time-based measurements must have valid start and end timestamps with the start preceding the end.
- Observer identity must be recorded for every behavioral observation to enable inter-rater reliability analysis.
- Target behaviors must correspond to defined behavioral objectives from an active treatment plan.

## Core Domain Concepts

### Symptom Tracking

Systematic recording of ADHD symptom manifestations across the three core dimensions:

- **Inattention Monitoring**: Tracking instances of difficulty sustaining attention, careless errors, failure to follow through on instructions, difficulty organizing tasks, avoidance of sustained mental effort, losing things, distractibility, and forgetfulness. Recorded by frequency count, duration measurement, or interval sampling.
- **Hyperactivity Monitoring**: Tracking instances of fidgeting, leaving seat when expected to remain seated, running or climbing inappropriately, difficulty with quiet leisure activities, being "on the go," and excessive talking. Recorded by frequency and duration across structured and unstructured settings.
- **Impulsivity Monitoring**: Tracking instances of blurting out answers, difficulty waiting turn, and interrupting or intruding on others. Recorded by frequency count with antecedent and consequence documentation.

### Behavior Charting Methods

Structured recording methodologies adapted for ADHD behavioral data:

- **Frequency Recording**: Counting the number of times a specific target behavior occurs within a defined observation period. Used for discrete behaviors like calling out in class, losing materials, or leaving seat.
- **Duration Recording**: Measuring the total time spent engaged in a target behavior. Used for sustained attention tasks, time-on-task measurement, and activity completion timing.
- **Interval Recording**: Dividing an observation period into equal intervals and recording whether the target behavior occurred during each interval. Used for behaviors that are difficult to count discretely, such as fidgeting or off-task behavior.
- **ABC (Antecedent-Behavior-Consequence) Recording**: Documenting the events preceding a target behavior, the behavior itself, and the consequences that follow. Used for functional behavior analysis to identify triggers and reinforcers.
- **Daily Report Card (DRC)**: A structured rating system where teachers evaluate specific target behaviors at defined intervals throughout the school day, providing data for home-school coordination.

### Executive Function Monitoring

Tracking performance across core executive function domains commonly impaired in ADHD:

- **Working Memory**: Monitoring the ability to hold and manipulate information during tasks, tracking errors related to forgetting instructions, losing track of multi-step processes, and difficulty with mental arithmetic.
- **Cognitive Flexibility**: Monitoring the ability to shift between tasks, adapt to changing rules, and transition between activities. Tracking perseverative errors and transition difficulty.
- **Inhibitory Control**: Monitoring the ability to suppress prepotent responses, resist distractions, and delay gratification. Tracking impulsive responses and difficulty with stop-signal tasks.
- **Planning and Organization**: Monitoring the ability to plan ahead, break tasks into steps, prioritize, and maintain organized materials and workspaces.
- **Time Management**: Monitoring time estimation accuracy, time awareness, ability to meet deadlines, and pacing during timed tasks.

### Cross-Setting Comparison

Analyzing behavioral data across different environments to identify setting-specific patterns:

- Home versus school symptom pattern comparison to identify environmental factors.
- Structured versus unstructured activity comparison to assess the role of external structure.
- Morning versus afternoon comparison to evaluate medication timing effects.
- Weekday versus weekend comparison to assess the impact of school-related demands.

## Data Collection Stakeholders

- **Parents/Caregivers**: Home setting observations, evening and weekend behavioral data, medication adherence notes.
- **Teachers**: Classroom behavioral data, academic task performance, peer interaction observations.
- **Clinicians**: Clinical session observations, standardized behavioral coding during assessment.
- **Self-Report**: For adolescents and adults, self-monitoring of symptoms, attention lapses, and executive function difficulties.
- **Coaches**: Coaching session observations, organizational skill performance, goal-directed behavior.

## Domain Events Produced

- **SignificantSymptomChangeDetected**: Triggers treatment plan review.
- **ExecutiveFunctionDeclineObserved**: Triggers clinical reassessment consideration.
- **BehaviorTargetAchieved**: Triggers treatment goal review.

## Relationships to Other Contexts

- **Partnership with Treatment Management**: Behavioral targets defined by treatment plans drive what is monitored. Monitoring data informs treatment plan adjustments.
- **Published Language to Outcomes Tracking**: Standardized symptom severity scores are published for treatment effectiveness evaluation.
- **Open Host Service from Medication Management**: Medication change events are consumed to correlate medication adjustments with behavioral changes.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Barkley, R. A. (2015). *Attention-Deficit Hyperactivity Disorder: A Handbook for Diagnosis and Treatment* (4th ed.). Guilford Press.
- DuPaul, G. J., & Stoner, G. (2014). *ADHD in the Schools* (3rd ed.). Guilford Press.
