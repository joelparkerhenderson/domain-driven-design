# Domain Events in Gastroenterology

## Overview

A domain event represents a significant occurrence within the domain that domain experts
care about. Domain events capture the fact that something important happened, expressed
in the ubiquitous language of the bounded context. In gastroenterology, domain events
signal clinically meaningful occurrences that may trigger actions within the same
bounded context or in other bounded contexts.

## Domain Event Characteristics

Domain events in the gastroenterology domain have the following characteristics:

1. Named in past tense using ubiquitous language (e.g., ProcedureCompleted, FlareDetected).
2. Immutable once created: they record what happened and cannot be changed.
3. Carry enough information for consumers to act without querying the source context.
4. Timestamped to establish when the occurrence took place.
5. Associated with the aggregate that raised them.

## Clinical Assessment Context Events

### AssessmentCompleted

Raised when a clinician completes a clinical assessment during a patient encounter.
Carries the encounter ID, patient ID, assessment findings, alarm feature flags, and
the preliminary diagnostic impression. Consumed by downstream contexts to determine
if specialized evaluation is needed.

### AlarmFeatureIdentified

Raised when a clinical assessment identifies an alarm feature such as dysphagia, GI
bleeding, unintended weight loss, or unexplained anemia. This event triggers urgent
diagnostic pathways. Consumed by the Endoscopic Procedures Context to prioritize
procedure scheduling.

### LabResultReceived

Raised when laboratory results are received and integrated into the clinical assessment.
Carries the encounter ID, lab panel type, and individual result values. Consumed by
the Hepatology Context (for liver function panels), the Inflammatory Disease Context
(for inflammatory markers), and the Nutrition Management Context (for nutritional
markers).

### ImagingReferralCreated

Raised when the assessment results in a referral for imaging studies. Carries the
referral details, clinical indication, and urgency level. Consumed by external
radiology integration through the anti-corruption layer.

## Endoscopic Procedures Context Events

### ProcedureScheduled

Raised when an endoscopic procedure is scheduled. Carries procedure type, indication,
patient preparation requirements, and scheduled date. Consumed by the Clinical
Assessment Context for patient notification and preparation instruction delivery.

### ProcedureCompleted

Raised when an endoscopic procedure is completed and documented. Carries the procedure
type, findings, therapeutic interventions performed, quality metrics, and specimen
collection details. Consumed by the Inflammatory Disease Context (for IBD surveillance
results), the Hepatology Context (for ERCP findings), and the Clinical Assessment
Context (for follow-up planning).

### PathologyResultReceived

Raised when pathology results from endoscopic biopsies are received. Carries the
specimen accession number, histological findings, and diagnostic interpretation.
Consumed by the Inflammatory Disease Context for disease activity assessment and
the Clinical Assessment Context for diagnostic refinement.

## Inflammatory Disease Context Events

### FlareDetected

Raised when disease activity scoring indicates an IBD flare. Carries the patient ID,
disease type, flare severity, current activity score, and baseline comparison. Consumed
by the Clinical Assessment Context for urgent visit scheduling and the Nutrition
Management Context for dietary adjustment.

### TherapyAdjusted

Raised when a biologic or immunomodulator therapy is adjusted. Carries the therapy
type, previous and new dosing, adjustment rationale, and monitoring schedule changes.
Consumed for pharmacy integration and monitoring coordination.

### RemissionAchieved

Raised when a patient achieves clinical or endoscopic remission. Carries the remission
type, final activity score, and maintenance therapy plan. Used for surveillance
scheduling and outcome tracking.

## Hepatology Context Events

### DiseaseProgressionDetected

Raised when liver disease staging indicates progression. Carries the previous and
current fibrosis stage, MELD score change, and new complication onset. Consumed by
the Nutrition Management Context for dietary optimization and the Clinical Assessment
Context for increased surveillance scheduling.

### TransplantCandidacyDetermined

Raised when a transplant evaluation concludes with a candidacy determination. Carries
the patient ID, MELD score, candidacy status, and listing recommendation. Consumed
for transplant center coordination.

### DecompensationEventOccurred

Raised when a cirrhotic patient experiences a decompensation event (new ascites,
variceal bleeding, encephalopathy). Carries the event type, severity, and immediate
management actions. Triggers urgent clinical response across contexts.

## Motility Disorders Context Events

### MotilityStudyCompleted

Raised when a manometry or pH monitoring study is completed and interpreted. Carries
the study type, key metrics, Chicago Classification or DeMeester score, and diagnostic
conclusion. Consumed by the Clinical Assessment Context for treatment planning.

### FunctionalDiagnosisEstablished

Raised when Rome IV criteria are satisfied for a functional GI disorder diagnosis.
Carries the specific disorder classification, criteria fulfillment details, and
recommended management approach.

## Nutrition Management Context Events

### NutritionalRiskIdentified

Raised when nutritional screening identifies a patient at risk for malnutrition.
Carries risk severity, contributing factors, and recommended intervention type.
Consumed by the Clinical Assessment Context for care plan integration.

### NutritionPlanModified

Raised when an active nutrition plan is modified. Carries the modification type,
previous and updated targets, and clinical rationale. Consumed for dietary counseling
coordination and supply management.

## Event Flow Patterns

Domain events in gastroenterology typically flow from upstream contexts to downstream
contexts, following clinical workflow. Assessment events flow to specialized contexts.
Procedure events flow to disease management contexts. Disease management events flow
back to assessment for ongoing care coordination. This creates a natural event-driven
clinical workflow.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 8: Domain Events.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 8: Domain Events.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 9: Communication Patterns.
