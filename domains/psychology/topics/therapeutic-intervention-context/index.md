# Therapeutic Intervention Context

## Overview

The Therapeutic Intervention Context is the bounded context responsible for the planning, delivery, and management of psychological treatment. It encompasses treatment planning, evidence-based therapy delivery, session management, therapeutic technique application, progress monitoring, and treatment completion. This context captures the clinical knowledge, therapeutic protocols, and professional judgment that guide the treatment of psychological conditions.

Therapeutic intervention is the primary service delivery function in most psychology practices. The domain model must accommodate the diversity of evidence-based approaches, the complexity of clinical decision-making, and the centrality of the therapeutic relationship while maintaining the structure needed for quality assurance and outcomes tracking.

## Core Aggregate: Treatment Plan

The Treatment Plan aggregate is the primary unit of consistency. It is rooted in the Treatment Plan entity and contains Treatment Goal entities, Intervention entities, Session entities, and Progress Note value objects.

The Treatment Plan aggregate enforces invariants including: every treatment goal must be linked to at least one evidence-based intervention; treatment plans must be reviewed at clinically appropriate intervals; session documentation must reference the goals addressed; and treatment modality changes must be justified by clinical rationale.

## Key Entities

Treatment Plan: The root entity representing the overall plan for a client's course of treatment. It carries the client reference, the primary and secondary diagnoses, the treatment modality, the planned duration, the review schedule, and the current status.

Session: An entity representing a discrete therapeutic encounter. It includes the date, duration, session number within the treatment protocol, the therapist, the session type, interventions applied, and the session status.

Intervention: An entity representing a specific therapeutic technique or activity applied within treatment. Examples include cognitive restructuring, behavioral activation, exposure exercises, mindfulness practices, and bilateral stimulation. Each intervention references the evidence-based protocol from which it derives.

## Key Value Objects

Treatment Goal: A specific, measurable objective that the treatment plan aims to achieve, including the target symptom or behavior, measurement criteria, and timeline.

Session Note Content: The clinical documentation of a session, including presenting concerns, interventions used, client response, risk assessment, and plan for the next session.

Diagnosis: The clinical diagnostic impression using DSM-5 or ICD-10 coding, with severity and course specifiers.

Therapeutic Alliance Rating: A quantified measure of the quality of the therapist-client working relationship, often assessed using validated instruments like the Working Alliance Inventory.

Case Formulation: The clinician's integrated theoretical understanding of the client's presenting problems, predisposing factors, precipitating events, perpetuating mechanisms, and protective factors.

## Evidence-Based Therapy Modalities

The context supports multiple evidence-based therapy modalities, each with its own protocol structure.

Cognitive Behavioral Therapy (CBT): A structured, time-limited approach that targets maladaptive thoughts and behaviors. The CBT protocol includes psychoeducation, cognitive restructuring, behavioral activation, exposure, and relapse prevention phases.

Acceptance and Commitment Therapy (ACT): A third-wave behavioral therapy that promotes psychological flexibility through acceptance, defusion, present-moment awareness, self-as-context, values clarification, and committed action.

Eye Movement Desensitization and Reprocessing (EMDR): A structured protocol for processing traumatic memories using bilateral stimulation. The eight-phase protocol includes history-taking, preparation, assessment, desensitization, installation, body scan, closure, and reevaluation.

## Domain Events

TreatmentPlanActivated: A treatment plan has been approved and treatment has begun.

SessionCompleted: A therapy session has been documented and finalized.

TreatmentGoalAchieved: A client has met the criteria for a specific treatment goal.

TreatmentPlanRevised: The treatment plan has been modified based on clinical progress or changing client needs.

TreatmentEpisodeClosed: Treatment has been formally concluded.

CrisisEventRecorded: A clinically significant safety concern has been identified during treatment.

## Session Management

Session management within this context handles scheduling logic that respects therapeutic protocols. CBT protocols typically require weekly sessions. EMDR protocols may require extended sessions of 90 minutes. Group therapy sessions have capacity limits. The domain model enforces protocol-appropriate session scheduling without coupling to external scheduling infrastructure.

## Integration with Other Contexts

The Therapeutic Intervention Context receives assessment summaries from the Psychological Assessment Context through an anti-corruption layer. It emits session completion events that the Outcomes Measurement Context consumes for progress tracking. It sends referrals to the Behavioral Analysis Context when systematic behavior assessment is needed. It consumes treatment effectiveness data from the Outcomes Measurement Context to inform clinical decisions.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Beck, J. S. (2020). Cognitive Behavior Therapy: Basics and Beyond (3rd ed.). Guilford Press.
- Hayes, S. C., Strosahl, K. D., & Wilson, K. G. (2012). Acceptance and Commitment Therapy (2nd ed.). Guilford Press.
- Shapiro, F. (2018). Eye Movement Desensitization and Reprocessing (EMDR) Therapy (3rd ed.). Guilford Press.
