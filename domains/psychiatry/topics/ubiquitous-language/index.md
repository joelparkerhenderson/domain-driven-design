# Ubiquitous Language in the Psychiatry Domain

## Overview

Ubiquitous language is the shared vocabulary used consistently by domain experts and developers within a bounded context. In psychiatry, establishing a precise ubiquitous language is critical because clinical terminology carries specific meanings that differ from everyday usage, and misinterpretation can have serious clinical consequences. The ubiquitous language ensures that software models faithfully represent psychiatric concepts as understood by clinicians.

## Principles

### Precision Over Convenience

Psychiatric terminology must be used with clinical precision in the domain model. The term "depression" in everyday language is vague, but in the Diagnostic Assessment Context, it must be modeled as a specific DSM-5-TR diagnosis (e.g., Major Depressive Disorder, Single Episode, Moderate, with Anxious Distress). The ubiquitous language enforces this precision by rejecting ambiguous terms in favor of clinically defined ones.

### Context-Specific Meaning

The same word can have different meanings across bounded contexts. "Assessment" in the Diagnostic Assessment Context means a structured psychiatric evaluation, while in the Outcomes Measurement Context it means administration of a standardized measurement instrument. The ubiquitous language is defined per bounded context, and explicit translation occurs at context boundaries.

### Clinician-Developer Collaboration

The ubiquitous language is developed through direct collaboration between psychiatrists, psychologists, social workers, and software developers. Domain experts contribute clinical accuracy, while developers ensure that terms can be represented faithfully in code. Terms that cannot be modeled precisely are refined through iterative discussion until both sides agree on their meaning and representation.

## Language by Context

### Diagnostic Assessment Context

Key terms include: psychiatric evaluation, mental status examination, DSM-5-TR diagnosis, diagnostic criteria, structured clinical interview (SCID-5), rating scale, PHQ-9, GAD-7, PANSS, YMRS, differential diagnosis, provisional diagnosis, rule-out diagnosis, diagnostic formulation, clinical impression, level of functioning, and Global Assessment of Functioning (GAF).

In this context, "evaluation" always means a comprehensive psychiatric assessment, not a general review or appraisal.

### Medication Management Context

Key terms include: prescription, medication regimen, psychotropic, antidepressant, antipsychotic, mood stabilizer, anxiolytic, stimulant, dosage, titration schedule, therapeutic range, drug level, drug interaction, contraindication, black box warning, side effect profile, pharmacogenomic panel, metabolizer status, and prior authorization.

In this context, "regimen" always means the complete set of active psychiatric medications for a patient, not a single prescription.

### Psychotherapy Context

Key terms include: therapy course, therapy session, therapeutic modality, cognitive behavioral therapy (CBT), dialectical behavior therapy (DBT), psychodynamic therapy, motivational interviewing, treatment protocol, session note, homework assignment, therapeutic alliance, treatment goal, progress indicator, treatment fidelity, and termination criteria.

In this context, "session" always means a scheduled psychotherapy appointment, not a login session or system session.

### Crisis Management Context

Key terms include: psychiatric emergency, crisis event, suicide risk assessment, Columbia Suicide Severity Rating Scale (C-SSRS), imminent risk, acute risk, chronic risk, involuntary hold, 5150 hold, 5250 hold, safety plan, means restriction, lethal means counseling, de-escalation, restraint, seclusion, and crisis disposition.

In this context, "hold" always means a legal involuntary psychiatric detention, not a general pause or delay.

### Rehabilitation Services Context

Key terms include: rehabilitation plan, psychosocial rehabilitation, supported employment, individual placement and support (IPS), job coaching, transitional housing, permanent supportive housing, social skills training, cognitive remediation, functional milestone, community integration, recovery goal, peer support, and independent living skills.

In this context, "recovery" follows the recovery model definition: a process of personal growth and community participation, not merely symptom remission.

### Outcomes Measurement Context

Key terms include: clinical outcome, outcome measure, measurement battery, clinically significant change, reliable change index, patient-reported outcome measure (PROM), functional assessment, quality indicator, benchmark, outcome report, treatment response, remission, relapse, measurement-based care, and routine outcome monitoring.

In this context, "response" means a predefined percentage reduction in symptom severity scores, not a general reaction.

## Language Governance

The ubiquitous language is maintained through a glossary reviewed and updated by a cross-functional team. New terms are proposed through a formal process requiring clinical validation and technical feasibility review. Deprecated terms are phased out with explicit migration timelines. The glossary is versioned alongside the domain model to ensure consistency.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 2 on ubiquitous language.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on developing ubiquitous language.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 2 on discovering domain knowledge.
- American Psychiatric Association. *DSM-5-TR*. APA Publishing, 2022.
