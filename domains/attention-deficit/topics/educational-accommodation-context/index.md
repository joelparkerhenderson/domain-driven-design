# Educational Accommodation Context: Attention-Deficit Domain

The Educational Accommodation Context manages school-based supports for students with ADHD, including IEP and 504 plan development, classroom accommodation strategies, academic support services, teacher consultation, and accommodation effectiveness tracking.

## Context Purpose

Students with ADHD frequently require educational accommodations to access the curriculum and demonstrate their knowledge effectively. This bounded context bridges the clinical and educational domains, translating diagnostic findings and clinical recommendations into educationally relevant supports. It operates within the regulatory frameworks of IDEA (Individuals with Disabilities Education Act) and Section 504 of the Rehabilitation Act, ensuring that accommodation plans meet legal requirements while addressing the student's functional needs in the classroom.

## Aggregate Root: Accommodation Plan

The Accommodation Plan aggregate encapsulates the complete set of educational supports for a student, whether structured as an IEP or a 504 plan. It maintains consistency across accommodation items, service assignments, review records, and implementation documentation.

Key invariants enforced by this aggregate:

- An active accommodation plan must have a current, valid eligibility determination.
- IEP plans must include measurable annual goals with progress reporting schedules as required by IDEA.
- 504 plans must document the disability, the major life activity affected, and the specific accommodations provided.
- Plans must have scheduled review dates within regulatory timelines (annual review for IEPs, periodic review for 504 plans).
- Each accommodation item must specify the responsible implementer (classroom teacher, special education teacher, related service provider).

## Core Domain Concepts

### Eligibility Determination

The process of establishing whether a student qualifies for educational accommodations:

- **IDEA Eligibility**: Students with ADHD may qualify under the "Other Health Impairment" (OHI) category if ADHD results in limited alertness to the educational environment, adversely affecting academic performance, and requiring specially designed instruction.
- **Section 504 Eligibility**: Students qualify when ADHD substantially limits one or more major life activities, including learning, reading, concentrating, thinking, and communicating. The threshold for 504 eligibility is lower than IDEA eligibility.
- **Evaluation Requirements**: Eligibility determination requires review of clinical assessment data, classroom performance data, teacher observations, and parent input. The Anti-Corruption Layer translates clinical diagnostic terminology into educational eligibility language.

### Accommodation Types for ADHD

Classroom accommodations categorized by the functional need they address:

- **Attention and Focus Accommodations**: Preferential seating (near teacher, away from distractions), reduced visual and auditory distractions, use of noise-canceling headphones, fidget tools, movement breaks, and chunked assignments with check-in points.
- **Organization Accommodations**: Assignment notebooks, color-coded materials, structured routines, visual schedules, checklists for multi-step tasks, organized workspace setup, and regular locker/desk clean-out support.
- **Time Management Accommodations**: Extended time for tests and assignments, visual timers, advance notice of transitions, reduced homework volume, and adjusted deadlines with structured planning support.
- **Assessment Accommodations**: Extended testing time (typically time-and-a-half), separate testing location with reduced distractions, frequent breaks during tests, and alternative assessment formats when appropriate.
- **Behavioral Support Accommodations**: Positive behavior reinforcement systems, daily report cards, access to a calm-down space, pre-arranged signals for redirection, and social skills group participation.
- **Instructional Accommodations**: Multi-sensory instruction, highlighted or outlined notes, verbal and written directions, frequent check-ins for understanding, and reduced copying requirements.

### IEP Components for ADHD

Individualized Education Program elements specific to ADHD management:

- **Present Levels of Performance**: Documentation of the student's current academic and functional performance, including how ADHD impacts learning across subject areas and school settings.
- **Measurable Annual Goals**: Specific, measurable goals addressing ADHD-related functional deficits (e.g., "Student will independently initiate assigned work within 2 minutes of direction 80% of the time").
- **Specially Designed Instruction**: Modified teaching approaches, such as explicit instruction in organizational skills, study strategies, or self-monitoring techniques.
- **Related Services**: Counseling, social work services, occupational therapy for fine motor or sensory needs, and speech-language services if language processing is impacted.
- **Supplementary Aids and Services**: Assistive technology, graphic organizers, text-to-speech software, and other tools supporting access to the curriculum.

### 504 Plan Components

Section 504 accommodation plan elements:

- **Disability Identification**: Documentation of ADHD diagnosis and how it substantially limits major life activities in the educational setting.
- **Accommodation Specifications**: Specific accommodations with implementation details, responsible personnel, and the settings where each accommodation applies.
- **Review Schedule**: Periodic review timeline (typically annual) to evaluate accommodation effectiveness and adjust as needed.

## Plan Lifecycle

1. **Referral**: Initiated by parent request, teacher referral, or clinical recommendation. Clinical assessment data is received through the Anti-Corruption Layer.
2. **Evaluation**: Educational evaluation conducted, integrating clinical findings with classroom performance data and teacher observations.
3. **Eligibility Determination**: Team decision on whether the student meets eligibility criteria under IDEA or Section 504.
4. **Plan Development**: Collaborative development of the IEP or 504 plan with parent, teacher, special education staff, and administrator participation.
5. **Implementation**: Accommodations are put in place across all relevant settings with responsible staff informed and trained.
6. **Progress Monitoring**: Ongoing tracking of accommodation implementation fidelity and student response.
7. **Annual Review**: Formal review and revision of the plan based on progress data, updated assessment information, and changing student needs.

## Domain Events Produced

- **AccommodationPlanCreated**: Triggers monitoring setup for accommodation effectiveness.
- **AccommodationReviewCompleted**: Triggers updated outcome measurement parameters.
- **AccommodationEffectivenessReported**: Provides data for treatment and outcomes contexts.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Individuals with Disabilities Education Act (IDEA), 20 U.S.C. 1400 et seq.
- Section 504 of the Rehabilitation Act of 1973, 29 U.S.C. 794.
- DuPaul, G. J., & Stoner, G. (2014). *ADHD in the Schools* (3rd ed.). Guilford Press.
