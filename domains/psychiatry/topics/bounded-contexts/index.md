# Bounded Contexts in the Psychiatry Domain

## Overview

A bounded context defines a logical boundary within which a particular domain model is consistent and complete. In the psychiatry domain, bounded contexts correspond to distinct areas of clinical responsibility, each with its own ubiquitous language, aggregate structures, and business rules. The psychiatry domain contains six bounded contexts that collectively represent the full scope of psychiatric care delivery.

## Context Definitions

### 1. Diagnostic Assessment Context

This context owns the psychiatric evaluation process. It manages structured clinical interviews, diagnostic criteria application (DSM-5-TR), standardized rating scales, and differential diagnosis workflows. The primary aggregates are the Psychiatric Evaluation and the Diagnostic Profile. Within this context, a "diagnosis" is a clinically validated classification assigned through a systematic evaluation process, not merely a billing code or a label.

Key responsibilities:
- Conducting and documenting psychiatric evaluations
- Applying DSM-5-TR diagnostic criteria systematically
- Administering and scoring standardized rating scales (PHQ-9, GAD-7, PANSS, YMRS)
- Managing differential diagnosis workflows
- Maintaining the patient's diagnostic history

### 2. Medication Management Context

This context owns the psychopharmacological treatment process. It manages prescriptions, therapeutic drug monitoring, drug interaction checking, and pharmacogenomic-guided prescribing. The primary aggregates are the Medication Regimen and the Prescription. Within this context, a "medication" encompasses the full pharmacological profile including mechanism of action, contraindications, and monitoring requirements.

Key responsibilities:
- Managing the prescription lifecycle (initiate, titrate, maintain, taper, discontinue)
- Checking drug-drug and drug-diagnosis interactions
- Monitoring therapeutic drug levels and side effects
- Incorporating pharmacogenomic test results into prescribing decisions
- Assessing polypharmacy risk

### 3. Psychotherapy Context

This context owns therapeutic intervention delivery. It manages modality selection, session scheduling and documentation, treatment protocol adherence, and therapeutic alliance tracking. The primary aggregates are the Therapy Course and the Therapy Session. Within this context, "therapy" refers specifically to structured psychotherapeutic interventions delivered according to evidence-based protocols.

Key responsibilities:
- Selecting appropriate therapeutic modality based on diagnosis and patient factors
- Managing session scheduling, attendance, and documentation
- Tracking treatment protocol adherence and fidelity
- Assigning and reviewing homework and between-session activities
- Monitoring therapeutic alliance quality

### 4. Crisis Management Context

This context owns psychiatric emergency response. It manages suicide risk assessment, involuntary hold procedures, safety planning, and de-escalation protocols. The primary aggregates are the Crisis Event and the Safety Plan. Within this context, "crisis" is defined as an acute psychiatric situation requiring immediate intervention to prevent harm.

Key responsibilities:
- Conducting structured suicide risk assessments (Columbia Protocol)
- Initiating and managing involuntary hold procedures
- Creating and maintaining safety plans
- Coordinating emergency contacts and resources
- Documenting de-escalation interventions

### 5. Rehabilitation Services Context

This context owns psychosocial rehabilitation planning and delivery. It manages supported employment, housing assistance, social skills training, cognitive remediation, and community reintegration. The primary aggregates are the Rehabilitation Plan and the Functional Milestone. Within this context, "rehabilitation" means structured interventions aimed at restoring functional capacity and community participation.

Key responsibilities:
- Developing individualized rehabilitation plans
- Managing supported employment placements and job coaching
- Coordinating housing and residential support services
- Delivering social skills training and cognitive remediation programs
- Tracking functional milestones and community participation

### 6. Outcomes Measurement Context

This context owns clinical outcome tracking and quality measurement. It manages standardized outcome instruments, functional assessment scales, patient-reported outcomes, and quality metrics. The primary aggregates are the Outcome Report and the Measurement Battery. Within this context, "outcome" is a quantified change in health status measured using validated instruments.

Key responsibilities:
- Administering outcome measurement instruments at defined intervals
- Calculating clinically significant change scores
- Aggregating patient-reported outcomes
- Producing quality metrics for regulatory and accreditation reporting
- Analyzing treatment effectiveness across populations

## Boundary Enforcement

Each bounded context maintains its own model of shared concepts. For example, the concept of "patient" exists in every context, but its representation differs:

- In Diagnostic Assessment, a patient has a diagnostic history and evaluation schedule.
- In Medication Management, a patient has a medication regimen and allergy profile.
- In Psychotherapy, a patient has a therapy course and session history.
- In Crisis Management, a patient has a risk profile and safety plan.

These different representations are intentional. Translation between contexts occurs through anti-corruption layers and published events, not through a shared data model.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on bounded context boundaries.
