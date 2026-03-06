# Behavioral Intervention Context: Autism Domain

The Behavioral Intervention Context manages the design, implementation, monitoring, and modification of behavioral interventions grounded in Applied Behavior Analysis (ABA), serving as a core bounded context in the autism domain.

## Purpose

ABA-based behavioral intervention is the most extensively researched and widely implemented evidence-based treatment for autism spectrum disorder. This bounded context manages the full lifecycle of behavioral services: conducting functional behavior assessments, designing behavior intervention plans, implementing skill acquisition programs through discrete trial training and natural environment teaching, collecting session-level data, analyzing behavioral trends, and modifying interventions based on data-driven decisions. It serves individuals across the lifespan, from early intensive behavioral intervention for young children to adult skills training.

## Domain Model

### Aggregates

The Intervention Plan Aggregate is the primary aggregate root, containing the behavior intervention plan (BIP) with its target behaviors, functional behavior assessment results, replacement behaviors, antecedent strategies, consequence strategies, and crisis protocols. It enforces the invariant that every target behavior must have at least one functionally equivalent replacement behavior and that the plan must reference a completed functional behavior assessment.

The Skill Acquisition Program Aggregate manages structured teaching programs for individual skills, including teaching procedures, prompt hierarchies, mastery criteria, and generalization plans. It enforces the invariant that mastery criteria must be defined before a program is activated.

The Therapy Session Aggregate captures session-level data including trial data, behavior incident records, and reinforcement data. It enforces the invariant that all trial data entries reference an active skill acquisition program and that session duration falls within authorized service hours.

### Key Entities

The Therapist entity represents ABA professionals (BCBA, BCaBA, RBT) with persistent identity across sessions and supervision cycles. The Intervention Episode entity represents a continuous period of service delivery bounded by authorization dates.

### Key Value Objects

BehaviorFrequency captures measured behavior rates. PromptLevel represents a position in the prompt hierarchy. MasteryCriteria defines the accuracy threshold, consecutive session count, and minimum trial count required for skill mastery. ReinforcementSchedule specifies the reinforcement delivery pattern.

## Functional Behavior Assessment (FBA)

The functional behavior assessment is the foundation of all behavior intervention planning. The FBA systematically identifies the antecedents and consequences that maintain a target behavior, leading to a hypothesis about the behavior's function. The four primary functions of behavior are:

- Attention: The behavior is maintained by social attention from others.
- Escape/Avoidance: The behavior is maintained by removal of demands or aversive stimuli.
- Tangible: The behavior is maintained by access to preferred items or activities.
- Automatic/Sensory: The behavior is maintained by the sensory stimulation it produces.

The FBA process includes indirect assessment (interviews, rating scales), descriptive assessment (ABC data collection in natural settings), and when appropriate, functional analysis (systematic manipulation of environmental variables).

## ABA Teaching Methodologies

### Discrete Trial Training (DTT)

DTT breaks complex skills into small, teachable components. Each trial consists of a discriminative stimulus (instruction), the individual's response, a consequence (reinforcement for correct responses or error correction for incorrect responses), and an inter-trial interval. Data is collected on each trial to monitor accuracy and guide prompt fading decisions.

### Natural Environment Teaching (NET)

NET uses naturally occurring situations, the individual's interests, and everyday routines to teach skills in context. Teaching opportunities are embedded in play, daily living activities, and social interactions. NET promotes generalization by teaching skills in the settings where they will be used.

### Verbal Behavior Approach

The verbal behavior approach, based on B.F. Skinner's analysis of verbal behavior, categorizes language by function: mands (requests), tacts (labels), echoics (imitation), intraverbals (conversational responses), and listener responding (following instructions). Skill acquisition programs target each verbal operant systematically.

## Data Collection and Analysis

Systematic data collection is fundamental to ABA practice. This context supports multiple data collection methods:

- Trial-by-trial recording: Documents the outcome of each discrete trial for skill acquisition programs.
- Frequency recording: Counts the number of times a target behavior occurs within an observation period.
- Duration recording: Measures how long a behavior lasts from onset to offset.
- Interval recording: Documents whether a behavior occurs during specified time intervals (partial interval, whole interval, momentary time sampling).
- ABC data collection: Records antecedent-behavior-consequence sequences for functional assessment.

The DataReviewService aggregates data across sessions to calculate trends, identify programs requiring modification, and support BCBA clinical decision-making during supervision reviews.

## Supervision and Oversight

BCBA supervisors oversee all ABA programming and therapy delivery. The supervision model ensures that skill acquisition programs and behavior plans are clinically sound, that RBTs implement procedures with fidelity, and that interventions are modified based on data analysis. Supervision requirements are governed by BACB ethical and professional standards.

## Domain Events

Key events produced by this context include InterventionPlanCreated, SkillMasteryAchieved, BehaviorIncidentRecorded, and InterventionPlanModified. SkillMasteryAchieved is consumed by the Outcomes Tracking and Educational Planning contexts to update milestone records and IEP goal progress.

## Integration Points

This context has a shared kernel with the Sensory Processing Context for ABC data involving sensory antecedents. It has a partnership with the Communication Support Context for functional communication training. It publishes behavioral data to the Educational Planning Context and Outcomes Tracking Context through published language interfaces.

## References

- Cooper, J. O., Heron, T. E., & Heward, W. L. (2020). *Applied Behavior Analysis* (3rd ed.). Pearson.
- Behavior Analyst Certification Board. (2022). *Ethics Code for Behavior Analysts*. BACB.
- Skinner, B. F. (1957). *Verbal Behavior*. Copley Publishing Group.
- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
