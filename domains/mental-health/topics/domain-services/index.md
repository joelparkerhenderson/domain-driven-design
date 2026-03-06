# Domain Services in the Mental Health Domain

## Overview

A domain service is a stateless operation that belongs to the domain layer
but does not naturally fit within a single entity or value object. Domain
services express significant domain concepts that involve coordinating
multiple aggregates, applying complex business rules that span entities, or
performing calculations that require information from several domain objects.

In the mental health domain, domain services handle clinical decision logic
that crosses aggregate boundaries within a bounded context: scoring algorithms,
interaction checking, risk calculations, and treatment matching.

## Domain Services by Bounded Context

### Clinical Assessment Context

**AssessmentScoringService**

Computes scores for standardized assessment instruments. This service is a
domain service rather than entity behavior because scoring algorithms vary
by instrument and may involve complex psychometric calculations that do not
belong inside the Assessment entity. The service takes the assessment
instrument type and the patient's responses, applies the instrument-specific
scoring algorithm (e.g., PHQ-9 summation with severity bands, GAD-7
threshold scoring), and returns an AssessmentScore value object.

**DiagnosticMatchingService**

Evaluates whether a patient's symptom profile meets DSM-5 diagnostic criteria
for specific disorders. This service coordinates between assessment results,
symptom duration, functional impairment data, and exclusion criteria. It does
not make diagnoses (that is a clinician's responsibility) but it surfaces
which diagnostic criteria are met, partially met, or not met to support
clinical decision-making.

### Treatment Planning Context

**ModalityRecommendationService**

Recommends therapeutic modalities based on the patient's diagnoses, severity
ratings, patient preferences, and evidence-based practice guidelines. This
service consults clinical practice guidelines (e.g., APA Practice Guidelines,
NICE Guidelines) to suggest modalities with the strongest evidence base for
the patient's specific diagnoses and severity levels. The clinician reviews
and selects from the recommendations.

**CareLevelDeterminationService**

Determines the appropriate care level (outpatient, intensive outpatient,
partial hospitalization, residential, inpatient) based on symptom severity,
functional impairment, risk factors, social support, and treatment history.
This service applies structured criteria (similar to the LOCUS or ASAM
criteria) to recommend a care level. The determination spans multiple
data points that no single entity owns.

### Therapy Management Context

**SessionSchedulingService**

Coordinates session scheduling based on treatment plan requirements (modality,
frequency), provider availability, patient preferences, and scheduling
constraints. This service spans the Session aggregate and external scheduling
data to find optimal appointment slots. It ensures that session frequency
aligns with the treatment plan's specifications.

**ProgressTrackingService**

Analyzes progress notes and session data across multiple sessions to identify
trends in patient engagement, intervention effectiveness, and therapeutic
alliance quality. This service reads from multiple Session aggregates to
produce a longitudinal view that no single session entity can provide.

### Crisis Intervention Context

**RiskCalculationService**

Computes a structured risk assessment by weighing risk factors, protective
factors, historical crisis data, and current clinical presentation. This
service applies validated risk assessment frameworks (e.g., Columbia Suicide
Severity Rating Scale methodology) to produce a RiskLevel value object.
The calculation integrates data from multiple sources within the crisis
episode and the patient's history.

**SafetyPlanningService**

Guides the creation and validation of safety plans based on the patient's
risk profile, available resources, and clinical best practices. This service
ensures safety plans contain required elements (warning signs, coping
strategies, social supports, professional contacts, emergency contacts,
means restriction) and validates that the plan is clinically appropriate
for the assessed risk level.

### Medication Management Context

**DrugInteractionCheckService**

Evaluates potential drug interactions when a new medication is prescribed or
a dosage is changed. This service compares the proposed medication against
the patient's current active prescriptions (retrieved from the
PrescriptionRepository) and a pharmacological knowledge base. It returns a
list of DrugInteraction value objects with severity ratings and recommended
actions. This is a domain service because it requires information from
multiple Prescription aggregates.

**TitrationPlanningService**

Creates titration schedules for psychotropic medications based on medication
pharmacokinetics, the patient's current dose, the target dose, and clinical
guidelines for dose escalation or tapering. This service produces a
TitrationSchedule entity that the Prescription aggregate incorporates.

### Outcomes Measurement Context

**ChangeDetectionService**

Calculates whether the change between measurement points is clinically
significant using established psychometric methods such as the Reliable
Change Index (RCI) or clinically significant change criteria. This service
compares consecutive scores, applies the instrument's reliability
coefficient and standard error of measurement, and determines whether the
change exceeds chance variation.

**EffectivenessReportingService**

Aggregates outcome data across measurement points, instruments, and treatment
goals to produce treatment effectiveness reports. This service coordinates
data from multiple OutcomeRecord aggregates and applies statistical
analyses that no single aggregate can perform alone.

## Design Principles

Domain services in the mental health domain follow these principles:

- Statelessness: domain services do not hold state between invocations.
  All necessary data is passed in as parameters or retrieved from
  repositories.
- Domain language: service methods are named using the ubiquitous language,
  not technical jargon.
- No infrastructure: domain services operate on domain objects only. They
  do not interact with databases, message queues, or external APIs.
- Single responsibility: each service handles one coherent domain operation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on domain services.
