# Treatment Management Context: Attention-Deficit Domain

The Treatment Management Context coordinates multimodal treatment planning for ADHD, encompassing behavioral therapy, parent training, coaching programs, psychoeducation, social skills training, and environmental modifications. This context owns the treatment plan lifecycle from creation through ongoing management to conclusion.

## Context Purpose

ADHD treatment is most effective when it combines multiple intervention modalities tailored to the individual's presentation type, severity, age, comorbidity profile, and functional impairment pattern. This bounded context manages the complex task of assembling, coordinating, and adapting these multimodal treatment plans. It translates diagnostic findings from the Clinical Assessment Context into actionable therapeutic interventions and tracks the progression of each treatment episode.

## Aggregate Root: Treatment Plan

The Treatment Plan aggregate maintains consistency across all active interventions for a given patient. It encapsulates treatment episodes, intervention assignments, therapy session records, and treatment goals within a single transactional boundary.

Key invariants enforced by this aggregate:

- A multimodal treatment plan must include at least two distinct treatment modalities.
- Each intervention within the plan must have at least one measurable behavioral objective.
- Treatment goals must specify baseline performance, target performance, measurement method, and review date.
- For preschool-age children (under 6), behavioral therapy must be the first-line intervention per AAP guidelines before medication is considered.
- Active treatment episodes cannot have conflicting session schedules.

## Core Domain Concepts

### Behavioral Therapy

Evidence-based behavioral interventions form the therapeutic foundation:

- **Applied Behavior Analysis (ABA) techniques**: Functional behavior assessment, reinforcement schedules, token economies, and contingency management applied to ADHD-related target behaviors.
- **Cognitive-Behavioral Therapy (CBT)**: Structured approach addressing maladaptive thought patterns, developing coping strategies, and building self-regulation skills. Particularly effective for adolescents and adults with ADHD.
- **Behavioral Parent Training (BPT)**: Structured programs teaching parents positive reinforcement techniques, consistent discipline strategies, effective command-giving, and environmental structuring. Programs include the Incredible Years, Triple P, and Parent-Child Interaction Therapy.

### Coaching Programs

ADHD coaching targets executive function skill development:

- **Organizational skills coaching**: Systematic development of planning, prioritization, time management, and task initiation abilities through structured coaching sessions and between-session practice.
- **Academic coaching**: School-focused coaching addressing homework management, study skills, note-taking strategies, and test preparation adapted for ADHD learning profiles.
- **Life skills coaching**: For adolescents and adults, coaching addresses daily living skills, financial management, relationship management, and career planning impacted by ADHD.

### Psychoeducation

Knowledge-based interventions for patients, families, and educators:

- **Patient psychoeducation**: Age-appropriate information about ADHD neurobiology, symptom management, self-advocacy, and strengths-based understanding of the condition.
- **Family psychoeducation**: Education for parents and siblings about ADHD, its impact on family dynamics, communication strategies, and stress management.
- **Teacher consultation**: Collaboration with educators to provide ADHD-informed classroom strategies and behavioral support techniques.

### Social Skills Training

Group-based interventions addressing social functioning deficits:

- **Social skills groups**: Structured group programs teaching conversational skills, perspective-taking, emotional regulation, and conflict resolution for children and adolescents with ADHD.
- **Peer relationship coaching**: Individual coaching addressing friendship development, social problem-solving, and managing impulsive social behaviors.

### Environmental Modifications

Structured changes to the physical and organizational environment:

- **Home environment structuring**: Establishing routines, reducing distractions, creating organizational systems, and designing homework environments optimized for ADHD.
- **Workplace accommodations**: For adults, designing work environments that support sustained attention, task completion, and time management.

## Treatment Plan Lifecycle

1. **Plan Creation**: Initiated by clinical assessment completion. Treatment modalities selected based on diagnostic findings, patient characteristics, and clinical practice guidelines.
2. **Intervention Assignment**: Specific therapists, coaches, and programs assigned to each treatment modality with scheduled session frequencies.
3. **Active Treatment**: Ongoing delivery of interventions with regular session documentation and progress notes.
4. **Progress Review**: Scheduled reviews (typically every 4-8 weeks) evaluating treatment response and adjusting the plan as needed.
5. **Plan Modification**: Addition, removal, or adjustment of treatment modalities based on progress review findings.
6. **Conclusion**: Treatment episodes conclude through goal achievement, planned step-down, or treatment discontinuation with documented rationale.

## Domain Events Produced

- **TreatmentPlanCreated**: Triggers monitoring setup and baseline outcome measurement.
- **TreatmentModalityChanged**: Triggers updated monitoring parameters.
- **TreatmentEpisodeConcluded**: Triggers final outcome evaluation.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- American Academy of Pediatrics. (2019). *Clinical Practice Guideline for the Diagnosis, Evaluation, and Treatment of ADHD in Children and Adolescents*.
- National Institute for Health and Care Excellence. (2018). *ADHD: Diagnosis and Management* (NG87).
