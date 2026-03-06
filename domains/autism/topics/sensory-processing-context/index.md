# Sensory Processing Context: Autism Domain

The Sensory Processing Context manages the assessment, planning, and management of sensory processing differences that significantly impact daily functioning for individuals with autism, serving as a supporting bounded context in the autism domain.

## Purpose

Sensory processing differences are a defining feature of autism spectrum disorder, recognized in the DSM-5 as hyper- or hyporeactivity to sensory input. This bounded context manages sensory profile assessments, sensory diet design, environmental modification recommendations, and sensory integration therapy protocols. It ensures that sensory interventions are individualized, evidence-informed, and integrated with behavioral and educational support across other bounded contexts.

## Domain Model

### Aggregates

The Sensory Profile Aggregate is the primary aggregate root, containing the comprehensive sensory processing assessment across all sensory modalities. It includes responses for visual, auditory, tactile, vestibular, proprioceptive, olfactory, and gustatory systems, quadrant classifications (seeking, avoiding, sensitivity, registration), and clinical interpretation. The invariant is that all sensory modalities must be assessed before the profile is considered complete.

The Sensory Diet Aggregate manages the personalized plan of sensory activities. It contains activity descriptions organized by time of day, target sensory systems, intensity levels, durations, and caregiver instructions. The invariant is that a sensory diet must reference a completed sensory profile and must address all sensory systems identified as areas of concern.

### Key Entities

The Occupational Therapist entity represents the OT professional who conducts assessments and designs interventions. The Sensory Environment entity represents specific physical settings (home, classroom, therapy room, community) where sensory characteristics are assessed and modifications are implemented.

### Key Value Objects

SensoryThreshold captures an individual's threshold for a specific sensory modality with its quadrant classification. SensoryActivity describes a single prescribed activity within a sensory diet. EnvironmentalFactor captures a specific environmental characteristic (lighting, noise level, texture, temperature, spatial density) with current and recommended levels.

## Sensory Profile Assessment

### Sensory Profile 2 (Dunn, 2014)

The Sensory Profile 2 is the primary standardized assessment tool used in this context. It evaluates sensory processing patterns through caregiver and self-report questionnaires across developmental age ranges. The assessment produces scores in four sensory processing quadrants based on Dunn's model:

- Seeking: High neurological threshold with active self-regulation strategy. The individual actively seeks out sensory input.
- Avoiding: Low neurological threshold with active self-regulation strategy. The individual actively avoids or limits sensory input.
- Sensitivity: Low neurological threshold with passive self-regulation strategy. The individual notices sensory input easily and may become distressed.
- Registration: High neurological threshold with passive self-regulation strategy. The individual may not detect or respond to sensory input.

### Sensory Modality Assessment

Each sensory modality is assessed individually to understand the individual's specific sensory processing pattern:

- Visual: Response to light intensity, movement, color, and visual complexity.
- Auditory: Response to sound volume, frequency, unexpected noises, and background noise.
- Tactile: Response to textures, temperature, pressure, and light touch.
- Vestibular: Response to movement, balance challenges, and positional changes.
- Proprioceptive: Response to deep pressure, joint compression, and heavy work activities.
- Olfactory: Response to smells of varying types and intensities.
- Gustatory: Response to tastes and food textures.

## Sensory Diet Design

A sensory diet is a personalized activity plan that provides scheduled sensory input throughout the day to help the individual maintain optimal arousal and self-regulation. The sensory diet is designed by an occupational therapist based on the completed sensory profile and is implemented by caregivers, teachers, and therapy staff.

Sensory diet activities are categorized by their primary sensory system target and their regulatory effect:

- Alerting activities: Increase arousal for individuals who are under-responsive (e.g., jumping, cold water, crunchy foods).
- Calming activities: Decrease arousal for individuals who are over-responsive (e.g., deep pressure, slow rocking, weighted blanket).
- Organizing activities: Help the individual achieve and maintain a balanced state (e.g., heavy work, rhythmic movement, oral motor input).

Activities are scheduled at strategic times throughout the day, with consideration for transitions, high-demand activities, and times when dysregulation is most likely.

## Environmental Modifications

Environmental modifications reduce sensory stressors in the individual's daily settings. The SensoryEnvironmentAssessmentService evaluates specific environments against the individual's sensory profile to identify mismatches and recommend modifications:

- Visual modifications: Adjusted lighting, reduced visual clutter, designated quiet visual spaces.
- Auditory modifications: Noise-reducing materials, headphones availability, predictable sound environments.
- Tactile modifications: Alternative seating options, texture choices, personal space boundaries.
- Spatial modifications: Defined movement areas, break spaces, reduced crowding.

## Integration with Behavioral Context

The Sensory Processing Context shares a kernel with the Behavioral Intervention Context because sensory triggers are frequently antecedents to challenging behaviors. When the Behavioral Intervention Context identifies a sensory antecedent through ABC data collection, the Sensory Processing Context provides sensory-informed antecedent strategies. The SensoryProfileCompleted event triggers updates to behavior intervention plans that include sensory antecedent management.

## Domain Events

Key events produced by this context include SensoryProfileCompleted, SensoryDietUpdated, and EnvironmentalModificationRecommended. These events are consumed by the Behavioral Intervention, Communication Support, and Educational Planning contexts.

## References

- Dunn, W. (2014). *Sensory Profile 2 Manual*. Pearson.
- Dunn, W. (1997). The impact of sensory processing abilities on the daily lives of young children and their families. *Infants and Young Children*, 9(4), 23-35.
- Ayres, A. J. (1972). *Sensory Integration and Learning Disorders*. Western Psychological Services.
- Miller, L. J., Anzalone, M. E., Lane, S. J., Cermak, S. A., & Osten, E. T. (2007). Concept evolution in sensory integration. *American Journal of Occupational Therapy*, 61(2), 135-140.
- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
