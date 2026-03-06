# Entities: Autism Domain

Entities are domain objects with a unique identity that persists over time, even as their attributes change, within the autism spectrum management domain.

## Purpose

In the autism domain, many objects must be tracked continuously across time as their state evolves. An individual receiving services maintains a persistent identity across years of diagnostic evaluations, therapy sessions, IEP reviews, and developmental milestones. Entities model these objects by assigning a unique, immutable identity that distinguishes them from all other objects of the same type, regardless of changes to their attributes.

## Cross-Context Entities

### Individual

The Individual entity represents a person receiving autism-related services. This entity exists across all bounded contexts but is modeled differently in each. In the Diagnostic Assessment Context, the Individual carries demographic data, developmental history, and diagnostic status. In the Behavioral Intervention Context, the Individual carries active intervention plans and behavioral baselines. Each context maintains its own projection of the Individual entity, synchronized through domain events and integration patterns.

Identity: A globally unique identifier assigned at intake that persists across all contexts and throughout the individual's lifetime of services.

### Caregiver

The Caregiver entity represents a parent, guardian, or family member who participates in the individual's care. Caregivers hold persistent identity because their involvement spans multiple contexts (signing IEP consent, participating in ABA parent training, implementing sensory diets at home) and evolves over time (custody changes, new family members joining care coordination).

## Diagnostic Assessment Context Entities

### Assessment Session

Represents a single scheduled evaluation session within a diagnostic evaluation. Tracks the date, duration, clinician, instruments administered, and clinical observations. The session identity persists because sessions may be referenced by subsequent diagnostic reports, insurance claims, and appeals.

### Clinician

Represents a diagnostic professional (developmental pediatrician, clinical psychologist, neuropsychologist) involved in the evaluation process. The clinician identity persists across multiple evaluations and is linked to credentials, licensure, and professional scope of practice.

## Behavioral Intervention Context Entities

### Therapist

Represents an ABA therapy professional (BCBA, BCaBA, RBT) who delivers behavioral interventions. Therapist identity persists across therapy sessions, supervision records, and credential maintenance cycles. Key attributes include certification level, supervised caseload, and competency areas.

### Intervention Episode

Represents a continuous period of ABA service delivery for one individual, spanning from authorization start through discharge or reauthorization. The episode identity persists because insurance authorizations, progress reports, and outcome measurements reference specific episodes.

## Sensory Processing Context Entities

### Occupational Therapist

Represents the OT professional who conducts sensory assessments and designs sensory interventions. Identity persists across multiple sensory profiles and sensory diet revisions for the same individual and across the therapist's full caseload.

### Sensory Environment

Represents a specific physical environment (home, classroom, therapy room, community setting) where sensory modifications are implemented. The environment identity persists because modifications are tracked and evaluated over time, and the same environment may be referenced by multiple sensory diet activities.

## Communication Support Context Entities

### Speech-Language Pathologist

Represents the SLP professional who conducts communication assessments and delivers speech-language therapy. Identity persists across therapy episodes, AAC evaluations, and interdisciplinary team participation.

### Communication Device

Represents a specific AAC device or system assigned to an individual. The device identity persists because device configurations evolve over time, warranty and maintenance records must be tracked, and usage data is collected longitudinally.

## Educational Planning Context Entities

### Educator

Represents a teacher, special education coordinator, or related service provider involved in the educational planning process. Identity persists across IEP cycles, school years, and classroom assignments.

### School Placement

Represents the specific school and classroom setting where an individual receives educational services. Placement identity persists because placement decisions are legally documented, transition between placements must be tracked, and placement history informs future decisions.

## Outcomes Tracking Context Entities

### Care Coordinator

Represents the professional responsible for coordinating services across contexts and monitoring overall progress. Identity persists across reporting periods and service episodes.

### Measurement Period

Represents a defined time interval during which outcome data is collected and analyzed. Identity persists because progress trends are calculated by comparing measurements across sequential periods.

## Entity Design Principles

- Identity is immutable: Once assigned, an entity's identity never changes, even if all other attributes are modified.
- Equality is identity-based: Two entity instances are equal if and only if they share the same identity, regardless of attribute values.
- Lifecycle tracking: Entities that span long time periods should include lifecycle state (active, on hold, discharged, archived) as a core attribute.
- Context-specific projections: The same real-world person or object may be represented as different entities in different bounded contexts, each carrying only the attributes relevant to that context.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 5 on entities.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 5 on entity design.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8 on entities and identity.
