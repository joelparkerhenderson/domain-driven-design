# Value Objects: Autism Domain

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity, used to model descriptive concepts within the autism spectrum management domain.

## Purpose

Many concepts in the autism domain are best modeled as value objects because they describe characteristics, measurements, or classifications rather than tracked individuals or processes. A DSM-5 diagnostic code, an ADOS-2 comparison score, or a sensory threshold level does not need a persistent identity -- it is fully defined by its attribute values. Value objects simplify the domain model by eliminating unnecessary identity management and enabling safe sharing and comparison by value.

## Diagnostic Assessment Context Value Objects

### DSM5DiagnosticCode

Represents a specific DSM-5 diagnostic classification. Attributes: code (e.g., 299.00), description (Autism Spectrum Disorder), edition (DSM-5 or DSM-5-TR). Two diagnostic codes are equal if their code and edition attributes match. Immutable because diagnostic codes are defined by the APA and do not change within an edition.

### ADOS2Score

Represents a score from the ADOS-2 instrument. Attributes: module number (1-4 or Toddler), algorithm domain (Social Affect, Restricted and Repetitive Behavior), raw score, comparison score (1-10), classification (non-spectrum, autism spectrum, autism). Immutable because a score, once computed from item-level data, does not change.

### SeverityLevel

Represents the DSM-5 severity level for autism spectrum disorder. Attributes: level (1, 2, or 3), description ("Requiring support", "Requiring substantial support", "Requiring very substantial support"), domain (social communication or restricted/repetitive behaviors). Immutable because severity levels are defined by the DSM-5 criteria.

### DevelopmentalAge

Represents an individual's developmental level in a specific domain. Attributes: domain (cognitive, language, motor, adaptive, social-emotional), age in months, assessment instrument used. Immutable because it represents a measurement at a point in time.

## Behavioral Intervention Context Value Objects

### BehaviorFrequency

Represents the measured frequency of a target behavior. Attributes: behavior name, count, observation period duration, rate (count per unit time). Immutable because it captures a measurement for a specific observation period.

### PromptLevel

Represents a level in the prompt hierarchy used during skill acquisition. Attributes: prompt type (full physical, partial physical, model, gestural, positional, verbal, independent), level number (ordinal position in the hierarchy). Immutable because the prompt hierarchy is a defined clinical protocol.

### MasteryCriteria

Represents the criteria that define when a skill has been mastered. Attributes: accuracy percentage threshold, number of consecutive sessions required, minimum number of trials per session. Immutable because mastery criteria are set when a skill acquisition program is designed and evaluated against collected data.

### ReinforcementSchedule

Represents the schedule governing reinforcement delivery. Attributes: schedule type (continuous, fixed ratio, variable ratio, fixed interval, variable interval), ratio or interval value, thinning plan. Immutable because it describes a specific reinforcement configuration.

## Sensory Processing Context Value Objects

### SensoryThreshold

Represents an individual's threshold for a specific sensory modality. Attributes: sensory system (visual, auditory, tactile, vestibular, proprioceptive, olfactory, gustatory), threshold classification (low registration, sensory seeking, sensory sensitivity, sensory avoiding), score. Immutable because it captures a measurement from a specific assessment.

### SensoryActivity

Represents a single activity within a sensory diet. Attributes: activity description, target sensory system, intensity level, recommended duration in minutes, time of day. Immutable because it describes a specific prescribed activity configuration.

### EnvironmentalFactor

Represents a specific environmental characteristic that affects sensory processing. Attributes: factor type (lighting, noise level, texture, temperature, spatial density), current level, recommended level, modification description. Immutable because it captures a specific environmental assessment finding.

## Communication Support Context Value Objects

### CommunicationLevel

Represents an individual's current communication ability level. Attributes: modality (verbal, gestural, pictorial, device-aided), expressive level, receptive level, assessment instrument used. Immutable because it captures a measurement at a point in time.

### AACDeviceType

Represents a category of AAC device or system. Attributes: category (no-tech, low-tech, mid-tech, high-tech), access method (direct selection, scanning, eye gaze), output type (visual only, voice output). Immutable because it describes a device classification.

### PECSPhase

Represents a phase in the PECS protocol. Attributes: phase number (1-6), phase name (e.g., "How to Communicate", "Distance and Persistence"), prerequisite skills, target skills. Immutable because PECS phases are defined by the published protocol.

## Educational Planning Context Value Objects

### IEPGoalStatus

Represents the progress status of an IEP goal at a specific reporting point. Attributes: goal identifier, reporting period, progress level (not introduced, emerging, progressing, mastered), data summary. Immutable because it captures a status at a point in time.

### AccommodationType

Represents a specific accommodation or modification. Attributes: category (presentation, response, setting, timing/scheduling), description, applicable settings. Immutable because it describes a specific accommodation configuration.

### ServiceDeliverySpec

Represents the specification for a related service. Attributes: service type (speech therapy, OT, ABA, counseling), frequency (sessions per week or month), duration per session in minutes, delivery setting (pull-out, push-in, consultation). Immutable because it describes a specific service authorization.

## Outcomes Tracking Context Value Objects

### MilestoneAchievement

Represents the achievement status of a developmental milestone. Attributes: milestone name, expected age range, actual achievement date (if achieved), status (not yet, emerging, achieved). Immutable because it captures a milestone observation at a point in time.

### EffectivenessRating

Represents a rating of treatment effectiveness for a specific intervention. Attributes: intervention type, measurement period, outcome metric, baseline value, current value, change percentage, clinical significance indicator. Immutable because it captures a calculated effectiveness measure.

## Value Object Design Principles

- Immutability: Value objects are never modified after creation. Changes produce new instances.
- Equality by value: Two value objects are equal if all their attributes are equal.
- Side-effect-free: Operations on value objects return new value objects without modifying existing ones.
- Self-validation: Value objects validate their own attribute values upon creation, rejecting invalid states.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 5 on value objects.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 6 on value object design.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8 on value objects and immutability.
