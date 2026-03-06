# Entities in the Gerontology Domain

## Overview

Entities are domain objects with a unique identity that persists over time, even as their attributes change. In the gerontology domain, entities represent the people, assessments, care plans, and clinical artifacts that must be tracked and referenced throughout the lifecycle of geriatric care. Unlike value objects, which are defined by their attributes, entities are defined by their identity: a specific patient, a specific assessment, or a specific care plan.

Evans (2003) emphasizes that identity management is a critical design decision. In gerontology, where an older adult may be tracked across decades of care, identity must be stable, unambiguous, and consistent across bounded contexts even when the entity's attributes differ between contexts.

## Patient Entity

The Patient entity exists in every bounded context but with different attributes and behaviors. In the Geriatric Assessment Context, the Patient entity includes demographics, medical history, and assessment history. In the Medication Management Context, the Patient entity includes renal function, hepatic function, allergy profile, and current medication list. Each bounded context models the Patient with the attributes relevant to its concerns, sharing only the patient identifier across contexts.

The Patient entity's identity is established at registration and persists through all care episodes, facility transfers, and provider changes. This stable identity enables longitudinal tracking of health status, functional decline, cognitive changes, and medication history over time.

## Geriatric Assessment Context Entities

### GeriatricAssessment Entity

The GeriatricAssessment entity represents a single comprehensive geriatric assessment event. It has a unique assessment identifier, a reference to the patient, the assessing clinician, the date of assessment, and the assessment status (initiated, in-progress, completed, reviewed). The entity's identity persists even as component evaluations are added and the status changes.

### FrailtyScreening Entity

The FrailtyScreening entity represents a specific frailty evaluation within a geriatric assessment. It tracks the screening instrument used, the date performed, the clinician conducting the screening, and the frailty classification result. Multiple frailty screenings for the same patient are distinct entities, enabling comparison over time.

### FallRiskEvaluation Entity

The FallRiskEvaluation entity captures a structured fall risk assessment. It includes the evaluation instrument (Morse Fall Scale, Tinetti Assessment), the date, the evaluator, the risk score, and the risk classification (low, moderate, high). Each evaluation is a distinct entity with its own identity and lifecycle.

## Care Coordination Context Entities

### CareTeamMember Entity

The CareTeamMember entity represents a healthcare professional or informal caregiver participating in an older adult's care team. It includes the member's role (geriatrician, nurse, social worker, pharmacist, family caregiver), contact information, and period of involvement. The entity's identity persists as the member's role or contact details change.

### CareGoal Entity

The CareGoal entity represents a specific clinical or functional goal within a care plan. It includes the goal description, target date, status (active, achieved, modified, discontinued), and the responsible provider. Goals evolve over time as the patient's condition and preferences change.

### CommunityResourceReferral Entity

The CommunityResourceReferral entity tracks a referral to an external community service. It includes the service type (meal delivery, transportation, adult day program), the referral date, status (pending, accepted, active, completed), and outcome. Each referral is independently tracked to ensure follow-through.

## Cognitive Health Context Entities

### DementiaStaging Entity

The DementiaStaging entity represents a specific dementia severity classification for a patient at a point in time. It includes the staging instrument used (Global Deterioration Scale, Clinical Dementia Rating), the stage assigned, the date of staging, and the clinician's notes. Multiple staging events for the same patient form a longitudinal record of cognitive decline.

### BehavioralIncident Entity

The BehavioralIncident entity records a specific behavioral or psychological symptom of dementia (BPSD) event. It includes the incident type (agitation, wandering, aggression), date and time, duration, triggers, interventions attempted, and outcome. Tracking individual incidents supports pattern analysis and intervention planning.

## Functional Independence Context Entities

### MobilityAidPrescription Entity

The MobilityAidPrescription entity represents the prescription or recommendation of a specific mobility device. It includes the aid type (walker, wheelchair, cane), the prescribing clinician, the fitting date, and the usage status. The entity tracks the aid from prescription through fitting, training, and ongoing use.

### HomeModificationRecommendation Entity

The HomeModificationRecommendation entity represents a specific recommended change to the patient's living environment. It includes the modification type (grab bar installation, ramp construction, lighting improvement), the priority level, the status (recommended, approved, completed), and the estimated cost.

## Medication Management Context Entities

### PrescribedMedication Entity

The PrescribedMedication entity represents a specific medication in a patient's regimen. It includes the medication name, dosage, route, frequency, prescribing clinician, start date, and status (active, discontinued, suspended). Each medication entry is a distinct entity, enabling granular tracking of prescription changes.

### DeprescribingCandidate Entity

The DeprescribingCandidate entity identifies a medication flagged for potential discontinuation. It includes the reason for deprescribing consideration (Beers criteria match, therapeutic duplication, no current indication), the risk-benefit assessment, and the deprescribing status (identified, discussed with patient, tapering, discontinued).

## End of Life Planning Context Entities

### SurrogateDecisionMaker Entity

The SurrogateDecisionMaker entity represents a person designated to make healthcare decisions on behalf of the patient. It includes the surrogate's relationship to the patient, contact information, legal authority documentation, and the scope of decision-making authority. The entity's identity persists through changes in contact details or legal status.

### GoalsOfCareConversation Entity

The GoalsOfCareConversation entity records a specific goals-of-care discussion. It includes the participants, the date, the topics discussed, the patient's expressed preferences, and the agreed-upon care direction. Each conversation is a distinct entity enabling longitudinal tracking of preference evolution.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
