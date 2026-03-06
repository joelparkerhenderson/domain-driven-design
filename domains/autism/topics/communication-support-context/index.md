# Communication Support Context: Autism Domain

The Communication Support Context manages communication assessment, augmentative and alternative communication (AAC) systems, speech-language therapy, and social communication interventions for individuals with autism, serving as a supporting bounded context in the autism domain.

## Purpose

Communication differences are a core feature of autism spectrum disorder. This bounded context manages the full spectrum of communication support, from comprehensive communication assessments through AAC device selection and configuration, PECS implementation, speech-language therapy goal setting and progress monitoring, and social communication interventions. It ensures that communication interventions are individualized, evidence-based, and coordinated with behavioral and educational goals across other bounded contexts.

## Domain Model

### Aggregates

The Communication Plan Aggregate is the primary aggregate root, containing the comprehensive communication intervention plan. It includes current communication abilities, communication goals across modalities, AAC recommendations, therapy approach specifications, and family training components. The invariant is that goals must be based on a documented communication assessment and that AAC recommendations must include a device trial period.

The AAC Configuration Aggregate manages the setup and customization of an AAC device or system for one individual. It includes vocabulary selection, display layout, access method, voice output settings, and customization parameters. The invariant is that the access method must match the individual's motor capabilities and that the vocabulary must include core words appropriate to the communication level.

### Key Entities

The Speech-Language Pathologist entity represents the SLP professional with persistent identity across therapy episodes and interdisciplinary team participation. The Communication Device entity represents a specific AAC device with tracked configuration history, usage data, and maintenance records.

### Key Value Objects

CommunicationLevel captures the individual's current communication ability across modalities. AACDeviceType classifies devices by technology level and access method. PECSPhase represents a phase in the six-phase PECS protocol.

## Communication Assessment

Comprehensive communication assessment evaluates the individual's abilities across multiple domains:

- Expressive communication: How the individual communicates wants, needs, thoughts, and feelings. This includes spoken language, gestures, sign language, picture exchange, and device-aided communication.
- Receptive communication: How the individual understands communication from others, including spoken language comprehension, gesture comprehension, and visual schedule understanding.
- Pragmatic language: How the individual uses communication in social contexts, including joint attention, conversational turn-taking, topic maintenance, and nonverbal communication.
- Communication functions: The purposes for which the individual communicates, including requesting, rejecting, commenting, greeting, and sharing information.

Assessment instruments include the Communication and Symbolic Behavior Scales (CSBS), Preschool Language Scales (PLS-5), Clinical Evaluation of Language Fundamentals (CELF-5), and informal communication sampling.

## Augmentative and Alternative Communication (AAC)

### AAC Feature Matching

The AACFeatureMatchingService coordinates the process of selecting appropriate AAC systems by matching individual characteristics with device features:

- Motor access assessment: Evaluates fine motor capabilities, visual-motor coordination, and alternative access methods (switch scanning, eye gaze tracking, head pointing).
- Cognitive-linguistic assessment: Determines appropriate symbol representation level (objects, photographs, line drawings, text) and vocabulary organization strategy.
- Communication needs assessment: Identifies priority communication functions and contexts to guide vocabulary selection and display design.

### AAC Device Categories

- No-tech AAC: Communication boards, picture cards, gestures, and sign language that require no electronic equipment.
- Low-tech AAC: Simple voice output devices (single message, sequential message) and static display communication boards.
- Mid-tech AAC: Devices with pre-programmed messages and limited customization.
- High-tech AAC: Tablet-based and dedicated speech-generating devices with dynamic displays, robust vocabulary systems, and extensive customization.

## Picture Exchange Communication System (PECS)

PECS is a structured AAC intervention developed specifically for individuals with autism and related developmental disabilities. The six-phase protocol teaches communication through picture exchange:

- Phase I (How to Communicate): The individual learns to exchange a single picture for a desired item with physical prompting.
- Phase II (Distance and Persistence): The individual learns to travel to a communication partner and to the communication book to make exchanges.
- Phase III (Picture Discrimination): The individual learns to select from multiple pictures to request specific items.
- Phase IV (Sentence Structure): The individual constructs multi-symbol sentences on a sentence strip (e.g., "I want" + item picture).
- Phase V (Responsive Requesting): The individual answers "What do you want?" using the sentence strip.
- Phase VI (Commenting): The individual learns to comment on environmental stimuli in response to questions like "What do you see?"

Each phase has specific mastery criteria and teaching procedures that the Communication Plan Aggregate tracks and enforces.

## Social Communication Interventions

Social communication interventions target the pragmatic language and social interaction differences associated with autism:

- Social skills groups: Structured peer interaction opportunities with explicit instruction in social rules and conventions.
- Video modeling: Use of video demonstrations to teach social behaviors, conversation skills, and emotional recognition.
- Social stories: Written narratives that describe social situations and expected responses to support social understanding.
- Peer-mediated intervention: Training neurotypical peers to support social interaction and communication with autistic individuals.

## Functional Communication Training (FCT)

FCT bridges the Communication Support Context and the Behavioral Intervention Context. When functional behavior assessment identifies that a challenging behavior serves a communication function (e.g., the individual hits to request a break), FCT teaches an alternative communicative response that serves the same function (e.g., using a "break" card). This partnership between contexts requires close coordination.

## Domain Events

Key events include CommunicationAssessmentCompleted, AACDeviceAssigned, and CommunicationGoalMet. These events inform the Educational Planning Context (IEP communication goals) and the Outcomes Tracking Context (communication milestones).

## References

- Bondy, A., & Frost, L. (2001). The Picture Exchange Communication System. *Behavior Modification*, 25(5), 725-744.
- Beukelman, D. R., & Light, J. C. (2020). *Augmentative and Alternative Communication* (5th ed.). Brookes Publishing.
- American Speech-Language-Hearing Association. (2006). *Guidelines for Speech-Language Pathologists in Diagnosis, Assessment, and Treatment of Autism Spectrum Disorders Across the Lifespan*.
- Carr, E. G., & Durand, V. M. (1985). Reducing behavior problems through functional communication training. *Journal of Applied Behavior Analysis*, 18(2), 111-126.
- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
