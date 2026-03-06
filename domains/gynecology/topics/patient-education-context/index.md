# Patient Education Context

## Overview

The Patient Education Context is a bounded context within the gynecology domain that supports patient engagement, health literacy, and informed decision-making. It manages the delivery of educational content, assessment of patient comprehension, facilitation of shared decision-making, and coordination of wellness programs and preventive care guidance.

## Scope and Responsibility

This context owns the education session model. Its responsibilities include:

- Assessing patient health literacy levels using validated tools to determine appropriate communication strategies.
- Matching educational content to clinical events, patient characteristics, and literacy levels.
- Delivering educational materials in formats appropriate to the patient's comprehension abilities.
- Facilitating shared decision-making by presenting treatment options with evidence-based outcome information.
- Documenting patient comprehension and any decisions reached during education sessions.
- Managing wellness program enrollment and participation tracking.
- Providing preventive care guidance aligned with clinical guidelines and the patient's personal risk profile.

## Ubiquitous Language

Key terms within this context:

- Education Session: A structured interaction in which educational content is delivered and patient comprehension is assessed.
- Health Literacy Assessment: An evaluation of a patient's ability to obtain, process, and act on health information.
- Shared Decision-Making: A collaborative process where clinicians and patients jointly evaluate options using evidence and patient values.
- Wellness Program: An organized initiative promoting preventive behaviors such as nutrition, exercise, and screening adherence.
- Preventive Care Guidance: Evidence-based recommendations for vaccinations, screenings, and lifestyle modifications.
- Content Delivery: The act of presenting educational material to a patient through selected modalities.
- Comprehension Assessment: An evaluation of whether the patient understood the information provided.
- Decision Outcome: The documented result of a shared decision-making session, including the patient's stated preference.
- Informed Consent: The process of ensuring the patient has sufficient understanding to make a voluntary, knowledgeable decision.

## Aggregate: Education Session

The Education Session is the primary aggregate root. It contains:

- ContentDelivery value objects recording what material was presented and through which modality.
- ComprehensionAssessment value objects capturing the patient's understanding level.
- DecisionOutcome value objects documenting decisions reached during shared decision-making.

Key invariants:

- A decision outcome cannot be recorded without a preceding comprehension assessment demonstrating adequate understanding.
- Content must be selected at or below the patient's assessed literacy level.
- An education session must be linked to a triggering clinical event or program enrollment.
- Comprehension must be assessed for each significant content topic delivered.

## Domain Events Published

- **EducationSessionCompleted**: Published when an education session is finished and comprehension is assessed. Consumed by all clinical contexts to record that education was provided.
- **LowComprehensionIdentified**: Published when a patient demonstrates inadequate comprehension, signaling the need for alternative educational approaches.
- **WellnessProgramEnrolled**: Published when a patient enrolls in a wellness program.

## Domain Events Consumed

- **DiagnosticImpressionFormed** (from Clinical Assessment): Triggers delivery of condition-specific educational content.
- **ReproductiveHealthPlanUpdated** (from Reproductive Health): Triggers delivery of updated reproductive health education.
- **SurgeryScheduled** (from Surgical Services): Triggers delivery of preoperative education.
- **SurgeryCompleted** (from Surgical Services): Triggers delivery of postoperative recovery education.
- **PregnancyConfirmed** (from Prenatal Care): Triggers initiation of prenatal education program.
- **HighRiskIdentified** (from Prenatal Care): Triggers delivery of high-risk pregnancy education.
- **AbnormalResultDetected** (from Screening Programs): Triggers delivery of result explanation and next-steps education.
- **ContraceptionMethodChanged** (from Reproductive Health): Triggers delivery of method-specific education.

## Domain Services

- **ContentMatchingService**: Selects appropriate educational content based on clinical event type, patient literacy level, language preference, and clinical context.
- **SharedDecisionSupportService**: Structures the decision-making process by presenting evidence-based options, formatting information appropriately, and documenting the decision.

## Value Objects

- LiteracyLevel, ContentTopic, ComprehensionScore.

## Integration Points

- Downstream from all clinical contexts. The Patient Education Context is a conformist; it consumes events from Clinical Assessment, Reproductive Health, Surgical Services, Prenatal Care, and Screening Programs without imposing its model on them.
- Uses an anti-corruption layer to translate clinical event data into educational trigger categories.

## Education Programs Modeled

- Prenatal education series covering nutrition, exercise, warning signs, labor preparation, and breastfeeding.
- Preoperative education covering procedure details, preparation steps, and recovery expectations.
- Contraception education covering method options, usage instructions, and side effect management.
- Screening result education explaining results, follow-up requirements, and emotional support resources.
- Wellness programs covering preventive health behaviors, screening adherence, and lifestyle modification.

## Subdomain Classification

Patient Education is a generic subdomain. Health literacy assessment and educational content delivery follow patterns common across medical specialties, though the content itself is gynecology-specific.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Institute of Medicine. Health Literacy: A Prescription to End Confusion. National Academies Press, 2004.
- ACOG. Committee Opinion No. 585: Health Literacy, 2014.
