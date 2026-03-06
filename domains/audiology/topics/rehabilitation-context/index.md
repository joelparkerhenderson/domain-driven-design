# Rehabilitation Context

## Overview

The Rehabilitation Context manages aural rehabilitation programs, auditory training activities, communication strategy instruction, and counseling services for individuals with hearing loss. This bounded context addresses the reality that hearing devices alone do not fully resolve communication difficulties. Effective hearing healthcare requires structured intervention that develops the patient's auditory skills, teaches compensatory strategies, and addresses the psychosocial impact of hearing loss.

## Strategic Importance

As a supporting subdomain, the Rehabilitation Context implements clinical recommendations from the Hearing Assessment Context and complements the amplification provided by the Device Management Context. Rehabilitation follows evidence-based protocols and outcome measurement frameworks. The domain model tracks intervention plans, monitors progress, and measures functional outcomes to demonstrate treatment effectiveness.

## Ubiquitous Language

Key terms within this bounded context include: aural rehabilitation, auditory training, communication strategy, speechreading, counseling, adjustment counseling, informational counseling, self-assessment, functional outcome, hearing handicap, hearing disability, activity limitation, participation restriction, significant other, communication partner, group rehabilitation, individual therapy, home exercise program, self-efficacy, and patient education.

## Aggregates

### Rehabilitation Plan

The Rehabilitation Plan is the central aggregate, rooted by the RehabilitationPlan entity. It contains RehabilitationGoal value objects (measurable objectives with target metrics and timeframes), InterventionActivity entities (scheduled therapeutic activities), and OutcomeMeasurement value objects (scores from standardized instruments). The aggregate enforces that all goals are measurable, interventions align with stated goals, and outcome instruments are administered at clinically appropriate intervals.

### Counseling Session

The Counseling Session aggregate captures individual or group counseling encounters. It contains session notes, topics addressed (informational counseling about hearing loss, adjustment counseling for psychosocial impact, communication strategies), and patient responses. The aggregate tracks the therapeutic relationship over time.

## Value Objects

RehabilitationGoal defines a specific, measurable, achievable, relevant, and time-bound objective. OutcomeMeasurement captures a standardized assessment result (APHAB, HHIE/HHIA, SSQ, IOI-HA, COSI) with subscale scores and normative comparisons. CommunicationStrategy describes a specific technique for improving communication (environmental modifications, visual cues, repair strategies). AuditoryTrainingExercise specifies an exercise type, difficulty level, and performance criteria.

## Domain Events

RehabilitationPlanCreated is published when a new plan is established, triggering documentation and scheduling. OutcomeMeasurementRecorded is published when a standardized measure is administered, enabling progress tracking. RehabilitationGoalAchieved is published when a patient meets a goal, triggering plan updates. PlanModified is published when intervention approaches are adjusted based on progress or changing needs.

## Domain Services

The OutcomeAnalysisService analyzes serial outcome measurements and determines whether changes are clinically significant using validated critical difference values for each instrument. The GoalSettingService generates candidate goals based on the patient's audiometric profile, device status, and self-reported communication difficulties. The ProgressTrackingService monitors intervention compliance and outcome trends.

## Business Rules

Rehabilitation plans must include at least one measurable goal with a defined timeframe. Outcome measures must be administered at baseline and at clinically appropriate follow-up intervals (typically three months, six months, and twelve months post-fitting). Changes in outcome scores must be evaluated against the critical difference for the instrument used to determine clinical significance. Rehabilitation plans must be reviewed and updated when the patient's audiometric or device status changes.

## Outcome Measurement Instruments

The domain model supports multiple standardized outcome instruments. The Abbreviated Profile of Hearing Aid Benefit (APHAB) measures self-perceived hearing aid benefit across four subscales: ease of communication, reverberation, background noise, and aversiveness. The Hearing Handicap Inventory for the Elderly or Adults (HHIE/HHIA) quantifies emotional and situational hearing handicap. The Speech, Spatial and Qualities of Hearing Scale (SSQ) assesses complex listening abilities. The International Outcome Inventory for Hearing Aids (IOI-HA) provides a brief global measure of hearing aid outcomes. The Client Oriented Scale of Improvement (COSI) captures patient-specific goals and their achievement.

## Rehabilitation Approaches

The domain model supports multiple rehabilitation approaches. Individual therapy provides personalized intervention tailored to the patient's specific communication challenges. Group rehabilitation offers peer support and shared learning through structured communication workshops. Computer-based auditory training provides self-paced exercises that develop phonemic discrimination and speech-in-noise perception. Communication partner training involves the patient's family and frequent communication partners in learning strategies that facilitate communication.

## Integration with Other Contexts

The Rehabilitation Context receives diagnostic data from the Hearing Assessment Context to understand the patient's hearing profile. It receives device fitting information from the Device Management Context to know the patient's current amplification. It publishes rehabilitation progress and outcomes to the Clinical Documentation Context for clinical reporting. When rehabilitation assessment reveals that the current device fitting is inadequate, the Rehabilitation Context raises an event that Device Management can respond to.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Montano, J. J., & Spitzer, J. B. (Eds.) (2020). Adult Audiologic Rehabilitation. 3rd Edition. Plural Publishing.
- Boothroyd, A. (2007). Adult Aural Rehabilitation: What Is It and Does It Work? Trends in Amplification.
