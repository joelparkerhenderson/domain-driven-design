# Domain Events in the Nutrition Domain

## Overview

A domain event is a record of something significant that happened within the domain. Domain events capture state changes that other parts of the system need to know about, enabling loose coupling between bounded contexts and supporting eventual consistency. In the nutrition domain, domain events are the primary mechanism for propagating information across the six bounded contexts, allowing each context to react to changes in others without direct dependencies.

## Domain Events by Bounded Context

### Nutritional Assessment Context

**AssessmentCompleted**
Raised when a nutritional assessment has been finalized with all required data (dietary recall, anthropometry, and optionally biomarkers). This event carries the assessment identifier, the subject identifier, a summary of key findings (caloric intake, macronutrient distribution, identified deficiencies), and the assessment date. Consumed by Clinical Nutrition (to inform diagnosis and therapy), Sports Nutrition (to adjust fueling plans), and Outcomes Tracking (to record assessment data points).

**NutrientDeficiencyIdentified**
Raised when nutrient analysis reveals an intake below the Estimated Average Requirement (EAR) or a biomarker below the reference range for a specific nutrient. This event carries the nutrient type, the measured value, the reference threshold, and the deficiency severity. Consumed by Clinical Nutrition (to trigger diagnosis workflows) and Meal Planning (to adjust nutrient targets).

**BiomarkerResultRecorded**
Raised when a new laboratory biomarker result is added to an assessment. Carries the biomarker type, value, reference range, and interpretation. Consumed by Outcomes Tracking (to update biomarker trend data) and Clinical Nutrition (to inform monitoring and evaluation).

### Meal Planning Context

**MealPlanActivated**
Raised when a meal plan transitions from draft to active status. Carries the plan identifier, the subject identifier, the plan duration, and summary nutritional targets. Consumed by Dietary Compliance (to establish the reference plan for adherence tracking) and Outcomes Tracking (to record the intervention start).

**MealPlanModified**
Raised when an active meal plan's nutritional targets or meal composition changes. Carries the plan identifier and a summary of changes. Consumed by Dietary Compliance (to update the compliance reference) and Outcomes Tracking (to record intervention adjustments).

**GroceryListGenerated**
Raised when a grocery list is produced from a meal plan. Carries the list identifier, the associated meal plan identifier, and the list period. This event is primarily internal to the Meal Planning context but may be consumed by external shopping or inventory systems.

### Clinical Nutrition Context

**NutritionDiagnosisEstablished**
Raised when a new nutrition diagnosis is formally recorded with a PES statement. Carries the diagnosis identifier, the patient identifier, the problem label, the etiology, and the supporting signs and symptoms. Consumed by Meal Planning (to apply new dietary constraints) and Outcomes Tracking (to record the clinical finding).

**DietPrescriptionIssued**
Raised when a disease-specific diet prescription is created or modified. Carries the prescription identifier, the patient identifier, diet type, and nutrient restrictions. Consumed by Meal Planning (to constrain meal plan design) and Dietary Compliance (to update compliance targets).

**EnteralFeedingStarted**
Raised when an enteral nutrition order is initiated. Carries the order identifier, formula specification, delivery parameters, and the patient identifier. Consumed by Outcomes Tracking (to record the intervention) and Dietary Compliance (to track formula administration adherence).

### Sports Nutrition Context

**FuelingPhaseTransitioned**
Raised when an athlete moves to a new training phase with different nutritional requirements. Carries the athlete identifier, the new phase type, updated macronutrient targets, and the transition date. Consumed by Meal Planning (to adjust the athlete's meal plan) and Dietary Compliance (to update adherence targets).

**REDSRiskDetected**
Raised when energy availability falls below the safe threshold of 30 kcal/kg FFM/day. Carries the athlete identifier, the calculated energy availability, and the risk severity. Consumed by Clinical Nutrition (to trigger clinical evaluation) and Outcomes Tracking (to flag the risk event).

**HydrationProtocolUpdated**
Raised when an athlete's hydration plan changes due to environmental conditions, training intensity changes, or sweat rate assessments. Carries the athlete identifier and new hydration parameters.

### Dietary Compliance Context

**ComplianceScoreCalculated**
Raised when adherence is calculated for a reporting period. Carries the subject identifier, the period, overall compliance percentage, and component scores. Consumed by Outcomes Tracking (to track adherence trends) and Clinical Nutrition (to inform therapy adjustments).

**ComplianceBarrierIdentified**
Raised when a pattern analysis identifies a specific barrier to dietary adherence (e.g., consistent non-compliance during evening meals or weekend overeating). Carries the barrier type, the pattern evidence, and suggested interventions. Consumed by Clinical Nutrition (to inform counseling) and Outcomes Tracking (to correlate barriers with outcomes).

### Outcomes Tracking Context

**OutcomeMilestoneReached**
Raised when a tracked outcome reaches a predefined goal (e.g., target weight achieved, biomarker normalized, body composition target met). Carries the subject identifier, the milestone type, the achieved value, and the date. This event is consumed by all other contexts to inform plan adjustments.

**TrendAlertTriggered**
Raised when a longitudinal trend analysis detects a concerning pattern (e.g., progressive weight loss exceeding safe rates, declining biomarker values). Carries the alert type, trend data, and recommended actions.

## Event Design Principles

Domain events in the nutrition domain are immutable records of past occurrences. They carry all data necessary for consumers to react without querying the producing context. Events are named in past tense to reflect that the action has already occurred. They include a timestamp, a correlation identifier for tracing related events, and the aggregate root identifier from which they originated.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
