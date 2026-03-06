# Domain Events in the Gerontology Domain

## Overview

Domain events represent significant occurrences within the domain that other parts of the system need to know about. They capture something that happened in the past and are named using past-tense verbs. In the gerontology domain, domain events are the primary mechanism for communication between bounded contexts, enabling loose coupling while ensuring that clinically important changes propagate to all relevant areas of the system.

Evans (2003) introduced domain events as a way to express side effects explicitly in the domain model. Vernon (2013) elaborates on their role in enabling eventual consistency between aggregates and bounded contexts. In geriatric care, domain events model the clinical workflow where an assessment finding triggers a care plan update, a medication change prompts reconciliation, or a goals-of-care conversation alters treatment direction.

## Geriatric Assessment Context Events

### AssessmentCompleted

Published when a comprehensive geriatric assessment has been finalized with all required evaluations. Contains the assessment identifier, patient identifier, assessment date, summary findings (frailty classification, fall risk level, nutritional status, cognitive screening result), and the assessing clinician. Consumed by Care Coordination (to initiate or update care plans), Cognitive Health (to trigger detailed screening if cognitive concerns are identified), Functional Independence (to establish functional baselines), and Medication Management (to initiate polypharmacy review).

### FrailtyStatusChanged

Published when a patient's frailty classification changes from a previous assessment. Contains the patient identifier, previous frailty classification, new frailty classification, and the date of change. Consumed by Care Coordination to adjust care intensity and by End of Life Planning to consider whether goals-of-care conversations should be initiated.

### FallRiskIdentified

Published when a patient is classified as moderate or high fall risk. Contains the patient identifier, risk score, risk classification, contributing factors, and the assessment date. Consumed by Functional Independence (to prioritize home modification assessment) and Care Coordination (to add fall prevention activities to the care plan).

### NutritionalDeficiencyDetected

Published when a nutritional assessment identifies malnutrition or malnutrition risk. Contains the patient identifier, nutritional screening score, BMI, weight change data, and the assessment date. Consumed by Care Coordination for dietary referral and Medication Management for appetite-affecting medication review.

## Care Coordination Context Events

### CarePlanCreated

Published when a new care plan is established for a patient. Contains the care plan identifier, patient identifier, care goals, assigned team members, and the creation date. Consumed by all bounded contexts to align their activities with the coordinated care plan.

### CareTransitionInitiated

Published when a patient begins a transition between care settings. Contains the patient identifier, origin setting, destination setting, transfer date, and the transition coordinator. Consumed by Medication Management (for medication reconciliation) and Functional Independence (for environmental assessment at the new setting).

### CaregiverBurdenAssessed

Published when a caregiver burden assessment identifies high strain. Contains the caregiver identifier, patient identifier, burden score, and the assessment date. Consumed by Care Coordination for respite care referral and community resource linkage.

## Cognitive Health Context Events

### CognitiveDeclineDetected

Published when cognitive testing reveals a significant decline from previous results. Contains the patient identifier, previous test scores, current test scores, the magnitude of change, and the testing date. Consumed by Care Coordination (to update the care plan), Medication Management (to review cognitive-impairing medications), and End of Life Planning (to consider early goals-of-care conversations while the patient retains decision-making capacity).

### DementiaStageProgressed

Published when a patient's dementia staging advances to a more severe classification. Contains the patient identifier, previous stage, new stage, staging instrument, and the staging date. Consumed by Care Coordination (for care intensity adjustment), Functional Independence (for safety assessment update), and End of Life Planning (for advance care planning).

### MemoryCarePlanEstablished

Published when a memory care plan is created or significantly revised. Contains the care plan identifier, patient identifier, behavioral interventions, safety precautions, and caregiver education components. Consumed by Care Coordination for integration with the overall care plan.

## Functional Independence Context Events

### FunctionalDeclineDetected

Published when ADL or IADL scores show a significant decrease from previous assessment. Contains the patient identifier, previous scores, current scores, specific domains affected, and the assessment date. Consumed by Care Coordination (to update care goals), Geriatric Assessment (to trigger reassessment), and End of Life Planning (if decline indicates approaching end of life).

### HomeModificationCompleted

Published when a recommended home modification has been installed. Contains the patient identifier, modification type, completion date, and safety assessment results. Consumed by Functional Independence (to update the environmental risk profile) and Care Coordination (to update care plan records).

### AssistiveTechnologyDeployed

Published when an assistive technology device is prescribed and set up for a patient. Contains the patient identifier, device type, deployment date, and training status. Consumed by Care Coordination for care plan documentation.

## Medication Management Context Events

### PolypharmacyReviewCompleted

Published when a medication review identifies potentially inappropriate medications. Contains the patient identifier, total medication count, number of Beers criteria medications identified, deprescribing recommendations, and the review date. Consumed by Care Coordination for care plan integration and the prescribing clinician for action.

### MedicationRegimenChanged

Published when medications are added, removed, or modified. Contains the patient identifier, the specific changes (added, discontinued, dose adjusted), the reason for change, and the effective date. Consumed by Care Coordination for medication reconciliation and Cognitive Health (if the medication change may affect cognition).

### AdverseDrugEventReported

Published when a suspected adverse drug reaction occurs. Contains the patient identifier, suspected medication, reaction description, severity, and the reporting date. Consumed by Medication Management for regimen review and Geriatric Assessment for risk profile update.

## End of Life Planning Context Events

### AdvanceDirectiveRecorded

Published when a patient completes or updates an advance directive. Contains the patient identifier, directive type, treatment preferences, surrogate decision maker, and the recording date. Consumed by all bounded contexts to ensure care delivery aligns with the patient's documented wishes.

### GoalsOfCareConversationCompleted

Published when a goals-of-care discussion concludes with documented preferences. Contains the patient identifier, conversation participants, topics discussed, stated preferences, and the conversation date. Consumed by Care Coordination for care plan alignment and all clinical contexts for treatment direction.

### HospiceReferralInitiated

Published when a hospice referral is made. Contains the patient identifier, hospice agency, referral date, prognosis, and the referring clinician. Consumed by Care Coordination (to transition the care plan), Medication Management (to shift to comfort-focused prescribing), and Functional Independence (to adjust goals to comfort and quality of life).

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
