# Aggregates in the Gynecology Domain

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that serves as the sole entry point for external references. In the gynecology domain, aggregates define the consistency boundaries within each bounded context, ensuring that clinical invariants are maintained during every state change.

## Purpose

Gynecological workflows involve complex clinical data that must remain internally consistent. A prenatal record cannot have a labor plan that contradicts its risk stratification. A surgical case cannot proceed without documented consent. Aggregates enforce these invariants by grouping related objects and controlling all modifications through the aggregate root.

## Aggregates by Bounded Context

### Clinical Assessment Context

**Clinical Encounter Aggregate**
- Root entity: ClinicalEncounter
- Contains: SymptomEvaluation, PhysicalFindings, DiagnosticWorkup, DiagnosticImpression
- Invariants: A diagnostic impression cannot be finalized without at least one completed evaluation. A workup must reference the presenting symptoms that motivated it.
- Lifecycle: Created at patient intake, progresses through evaluation stages, finalized when diagnostic impression is recorded.

### Reproductive Health Context

**Reproductive Health Plan Aggregate**
- Root entity: ReproductiveHealthPlan
- Contains: FertilityAssessment, ContraceptionPlan, MenstrualHistory, HormonalProfile, TreatmentProtocol
- Invariants: A contraception plan must be consistent with documented medical contraindications. Fertility assessment results must be current within the defined validity period.
- Lifecycle: Created when a patient establishes reproductive health goals, updated at each relevant visit, closed when goals are achieved or care is transferred.

### Surgical Services Context

**Surgical Case Aggregate**
- Root entity: SurgicalCase
- Contains: SurgicalConsent, OperativePlan, PerioperativeProtocol, PostoperativeFollowUp
- Invariants: Surgery cannot be scheduled without documented consent. The operative plan must specify the surgical approach and procedure type. Postoperative follow-up cannot be initiated before surgery completion.
- Lifecycle: Created at referral, advances through consent and planning, executed during surgery, closed after postoperative recovery is complete.

### Prenatal Care Context

**Prenatal Record Aggregate**
- Root entity: PrenatalRecord
- Contains: PrenatalVisit (collection), UltrasoundAssessment (collection), RiskStratification, LaborPlan
- Invariants: Gestational age must be consistent across all assessments. Risk stratification must be updated when new risk factors are identified. Labor plan must reflect the current risk level.
- Lifecycle: Created at pregnancy confirmation, accumulates visit data across trimesters, closed at delivery or pregnancy conclusion.

### Screening Programs Context

**Screening Episode Aggregate**
- Root entity: ScreeningEpisode
- Contains: ScreeningOrder, ScreeningResult, FollowUpPathway
- Invariants: A result cannot be recorded without a corresponding order. An abnormal result must trigger a follow-up pathway. The screening type must match the ordered test.
- Lifecycle: Created when screening is ordered, updated when results arrive, closed when follow-up is complete or result is normal.

### Patient Education Context

**Education Session Aggregate**
- Root entity: EducationSession
- Contains: ContentDelivery, ComprehensionAssessment, DecisionOutcome
- Invariants: A decision outcome cannot be recorded without a preceding comprehension assessment. Content must be appropriate for the patient's assessed literacy level.
- Lifecycle: Created when educational content is triggered, completed when comprehension is assessed and any decisions are documented.

## Aggregate Design Principles

1. Keep aggregates small. Each aggregate should enforce only the invariants that must be immediately consistent.
2. Reference other aggregates by identity, not by direct object reference.
3. Design aggregate boundaries around true consistency requirements, not convenience.
4. Use domain events to coordinate between aggregates that do not require immediate consistency.
5. The aggregate root is the only object that external code may hold a reference to.

## Consistency Boundaries

Within a single aggregate, all invariants are enforced within a single transaction. Between aggregates, eventual consistency is achieved through domain events. For example, when a ScreeningEpisode records an abnormal result, it publishes an AbnormalResultDetected event. The Clinical Assessment Context eventually creates a new ClinicalEncounter in response, but this occurs in a separate transaction.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on aggregate patterns.
