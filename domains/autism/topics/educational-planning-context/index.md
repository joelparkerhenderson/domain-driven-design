# Educational Planning Context: Autism Domain

The Educational Planning Context manages the development, implementation, and review of educational programs for individuals with autism in school settings, serving as a generic bounded context in the autism domain.

## Purpose

Education is a fundamental service for individuals with autism, governed by federal and state regulations that mandate appropriate educational programming. This bounded context manages Individualized Education Programs (IEPs), transition planning for post-secondary life, inclusive education strategies, classroom accommodation plans, and paraprofessional support coordination. It ensures that educational plans are legally compliant, individualized, measurable, and aligned with clinical intervention goals from other bounded contexts.

## Domain Model

### Aggregates

The IEP Document Aggregate is the primary aggregate root, containing the complete Individualized Education Program. It includes present levels of academic achievement and functional performance (PLAAFP), measurable annual goals, short-term objectives or benchmarks, related services, supplementary aids and services, accommodations, modifications, and placement determination. The invariant is that the IEP must include measurable annual goals, specify related services with frequency/duration/location, and be reviewed at least annually.

The Transition Plan Aggregate manages post-secondary transition planning components, including transition assessments, post-secondary goals (education/training, employment, independent living), transition services, and agency linkages. The invariant is that transition planning must begin no later than the IEP in effect when the student turns 16, and post-secondary goals must be based on age-appropriate transition assessments.

### Key Entities

The Educator entity represents teachers, special education coordinators, and related service providers with persistent identity across IEP cycles and school years. The School Placement entity represents the specific school and classroom setting with tracked placement history.

### Key Value Objects

IEPGoalStatus captures goal progress at a specific reporting point. AccommodationType describes a specific accommodation by category (presentation, response, setting, timing/scheduling). ServiceDeliverySpec specifies a related service with type, frequency, duration, and delivery setting.

## IEP Development Process

The IEP development process follows the legally mandated cycle defined by IDEA:

### Eligibility Determination

Before an IEP is developed, the student must be found eligible for special education services. For students with autism, eligibility is typically based on the diagnostic evaluation from the Diagnostic Assessment Context, supplemented by educational assessments. The anti-corruption layer translates clinical diagnostic information into educational eligibility determination language.

### Present Levels of Performance (PLAAFP)

The PLAAFP section documents the student's current academic achievement and functional performance across all relevant domains. It draws on data from classroom observations, standardized educational assessments, teacher reports, and clinical data from the Behavioral Intervention, Sensory Processing, and Communication Support contexts (received through published language interfaces).

### Annual Goals and Objectives

Annual goals must be measurable and address each area of need identified in the PLAAFP. Goals may address academic skills, functional life skills, communication, social skills, behavioral regulation, sensory management, and vocational skills. Short-term objectives or benchmarks break annual goals into incremental steps. The IEPGoalAlignmentService ensures alignment between IEP goals and active clinical intervention goals.

### Related Services

Related services support the student in benefiting from special education. For students with autism, common related services include:

- Speech-language therapy: Provided by SLPs, informed by the Communication Support Context.
- Occupational therapy: Provided by OTs, informed by the Sensory Processing Context.
- ABA services: Provided by BCBAs/RBTs, informed by the Behavioral Intervention Context.
- Counseling: Social-emotional support for mental health needs.
- Paraprofessional support: One-to-one or shared aide support in the classroom.

Each related service is specified with frequency (sessions per week or month), duration (minutes per session), and delivery setting (pull-out, push-in, or consultation).

### Accommodations and Modifications

Accommodations change how instruction is delivered or how the student demonstrates learning without altering the content standards. Modifications change what the student is expected to learn. Common accommodations for students with autism include extended time, preferential seating, visual schedules, sensory breaks, and modified assignment formats. Environmental modifications are informed by the Sensory Processing Context.

### Placement Determination

Placement is determined based on the least restrictive environment (LRE) principle, which requires that students with disabilities be educated with non-disabled peers to the maximum extent appropriate. Placement options range from full inclusion in general education with supports, to resource room services, to self-contained special education classrooms, to specialized schools.

## Transition Planning

Transition planning prepares students with autism for life after high school. The TransitionReadinessAssessmentService evaluates readiness across three post-secondary goal areas:

- Education/Training: Community college, vocational training, continuing education, or university.
- Employment: Competitive integrated employment, supported employment, or sheltered workshop.
- Independent Living: Housing, transportation, self-care, community participation, and self-advocacy.

Transition assessments include interest inventories, vocational evaluations, adaptive behavior assessments, and self-determination assessments. Transition services identify the instruction, community experiences, and agency linkages needed to achieve post-secondary goals.

## Inclusive Education Strategies

Inclusive education integrates students with autism into general education classrooms with appropriate supports. Strategies include universal design for learning (UDL) principles, peer support arrangements, co-teaching models, visual supports, structured routines, and positive behavior support.

## Domain Events

Key events include IEPCreated, IEPReviewed, and TransitionPlanActivated. These events are consumed by all intervention contexts and the Outcomes Tracking Context.

## References

- Individuals with Disabilities Education Improvement Act. (2004). 20 U.S.C. 1400 et seq.
- Yell, M. L. (2019). *The Law and Special Education* (5th ed.). Pearson.
- Turnbull, H. R., Turnbull, A. P., Wehmeyer, M. L., & Shogren, K. A. (2020). *Exceptional Lives: Practice, Progress, and Dignity in Today's Schools* (9th ed.). Pearson.
- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
