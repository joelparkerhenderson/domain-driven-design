# Entities in the Gynecology Domain

## Overview

An entity is a domain object defined by its unique identity rather than its attributes. Two entities with identical attributes but different identities are considered distinct. In the gynecology domain, entities represent clinical objects that persist over time, change state, and must be tracked individually throughout their lifecycle.

## Purpose

Gynecological care involves tracking individual patients, encounters, surgical cases, pregnancies, and screening episodes over time. Each of these must be uniquely identifiable and distinguishable from all others, even when their attributes are similar. Entities provide this identity-based tracking, ensuring that a specific patient's prenatal record is never confused with another patient's record regardless of how similar their clinical data may be.

## Key Entities by Bounded Context

### Clinical Assessment Context

**ClinicalEncounter**
- Identity: Unique encounter identifier assigned at intake.
- Key attributes: encounter date, presenting complaint, encounter status (open, in-progress, completed).
- Behavior: Records symptom evaluations, captures physical findings, initiates diagnostic workups, produces diagnostic impressions.
- Lifecycle: Transitions from open to in-progress as evaluations are performed, to completed when a diagnostic impression is finalized.

**DiagnosticWorkup**
- Identity: Unique workup identifier linked to a clinical encounter.
- Key attributes: ordered tests, pending results, completed results, workup status.
- Behavior: Tracks ordered diagnostic tests, receives results, supports clinical reasoning toward a diagnostic impression.

### Reproductive Health Context

**ReproductiveHealthPlan**
- Identity: Unique plan identifier linked to a patient.
- Key attributes: reproductive goals, current contraceptive method, active treatments, plan status.
- Behavior: Records fertility assessments, updates contraception plans, tracks menstrual history, manages treatment protocols.

**TreatmentProtocol**
- Identity: Unique protocol identifier.
- Key attributes: condition being treated, prescribed interventions, response tracking, protocol status.
- Behavior: Defines treatment steps, tracks adherence, records clinical responses.

### Surgical Services Context

**SurgicalCase**
- Identity: Unique case identifier assigned at referral.
- Key attributes: procedure type, surgical approach, case status (referred, consented, scheduled, completed, closed).
- Behavior: Manages consent documentation, defines operative plans, tracks perioperative protocol compliance, records surgical outcomes.

### Prenatal Care Context

**PrenatalRecord**
- Identity: Unique record identifier linked to a specific pregnancy.
- Key attributes: estimated due date, current gestational age, risk level, record status.
- Behavior: Accumulates prenatal visit data, incorporates ultrasound assessments, updates risk stratification, manages labor planning.

**PrenatalVisit**
- Identity: Unique visit identifier within a prenatal record.
- Key attributes: visit date, gestational age at visit, vital signs, clinical observations, visit type (routine, urgent).
- Behavior: Records clinical measurements, identifies new risk factors, triggers risk re-stratification when warranted.

### Screening Programs Context

**ScreeningEpisode**
- Identity: Unique episode identifier.
- Key attributes: screening type, order date, result status, follow-up status.
- Behavior: Tracks the lifecycle from ordering through result receipt and follow-up disposition.

### Patient Education Context

**EducationSession**
- Identity: Unique session identifier.
- Key attributes: session type, content topics covered, comprehension level, session status.
- Behavior: Records content delivery, assesses patient comprehension, documents shared decisions.

## Entity Design Principles

1. Identity is assigned at creation and never changes throughout the entity's lifecycle.
2. Entities encapsulate behavior. State changes occur through well-defined methods that enforce invariants.
3. Equality is based on identity, not on attribute comparison.
4. Entities within an aggregate can only be accessed through the aggregate root.
5. Entity lifecycle transitions are explicit and correspond to meaningful clinical workflow stages.

## Identity Generation

Identity strategies in the gynecology domain include:

- System-generated UUIDs for internal entities such as clinical encounters and screening episodes.
- Medically meaningful identifiers where clinical standards require them, such as prenatal record numbers.
- Composite identifiers where an entity's identity is scoped within its aggregate root.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entity design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on entity patterns.
