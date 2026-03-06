# Pain Management Context

## Overview

The Pain Management Context addresses the multimodal approach to pain that is a central concern for nearly all rheumatology patients. Pain in rheumatic diseases arises from inflammation, joint damage, central sensitization, and psychosocial factors. This context manages pain assessment using validated instruments, coordinates pharmacologic and non-pharmacologic interventions, facilitates physical therapy and occupational therapy referrals, and tracks functional outcomes. It operates as a supporting subdomain that serves patients across all disease categories in the rheumatology domain.

## Context Boundaries

The Pain Management Context owns pain assessment, multimodal intervention planning, therapy referral coordination, and functional outcome tracking related to pain and function. It does not manage disease-modifying therapy (owned by the Autoimmune Disease and Biologic Therapy contexts), perform clinical assessments (owned by the Clinical Assessment Context), or track disease-specific outcome measures (owned by the Outcomes Tracking Context).

The boundary reflects the clinical reality that pain management in rheumatology is a distinct discipline requiring coordination with physical and occupational therapists, often at external facilities. Pain management strategies apply across disease categories: an RA patient and an OA patient may follow similar pain management plans even though their disease management differs fundamentally.

## Aggregate: PainManagementPlan

The PainManagementPlan aggregate root coordinates all pain-related interventions for a patient. It contains PainAssessment value objects (baseline and follow-up pain measurements), PharmacologicIntervention entities (NSAID, corticosteroid, and analgesic prescriptions), TherapyReferral entities (referrals to physical and occupational therapy), FunctionalGoal value objects (measurable rehabilitation targets), and PatientEducation entities (self-management instruction records).

The aggregate enforces the invariant that every pain management plan must include at least one non-pharmacologic intervention. This rule reflects the rheumatology best practice that medications alone are insufficient for optimal pain control. The aggregate also enforces that therapy referrals must include specific, measurable functional goals.

## Pain Assessment

Pain assessment uses validated scales that capture multiple dimensions of the pain experience. The Visual Analog Scale (VAS) captures pain intensity on a 0-100 mm continuous scale. The Numeric Rating Scale (NRS) captures intensity on a 0-10 integer scale. The McGill Pain Questionnaire captures pain quality using descriptive terms organized by sensory, affective, and evaluative dimensions.

The PainLevel value object records the assessment scale used, the numeric score, pain location (joint-specific or generalized), pain character (aching, sharp, burning, throbbing, stiffness), temporal pattern (constant, intermittent, morning stiffness with duration), and aggravating and alleviating factors. Pain assessments are repeated at defined intervals to track response to interventions.

Morning stiffness duration is a particularly important measure in inflammatory arthritis. The PainLevel value object captures morning stiffness in minutes, with duration exceeding 30 minutes suggestive of inflammatory disease and duration exceeding 60 minutes associated with active inflammatory arthritis.

## Pharmacologic Interventions

The PharmacologicIntervention entity manages pain-specific medications within the plan. NSAIDs (ibuprofen, naproxen, celecoxib) are used for inflammatory and mechanical pain with documentation of cardiovascular and gastrointestinal risk assessment. Corticosteroids (prednisone, methylprednisolone) are used for acute flare management with taper schedules documented. Acetaminophen is used as baseline analgesia for osteoarthritis. Topical agents (diclofenac gel, capsaicin) are used for localized joint pain.

The context tracks medication interactions with disease-modifying therapy managed in other contexts. It does not duplicate DMARD management but documents pain-specific medications and their interaction considerations.

## Physical Therapy Referrals

Physical therapy is a cornerstone of rheumatology pain management. The TherapyReferral entity for physical therapy includes the referring diagnosis, specific functional limitations (reduced grip strength, limited range of motion, impaired gait, deconditioning), therapy goals (increase grip strength by a defined amount, improve knee flexion to a specific degree, walk a target distance without assistive device), and the requested frequency and duration of sessions.

Physical therapy approaches in rheumatology include range of motion exercises, strengthening programs (especially for periarticular muscles), aquatic therapy (particularly beneficial for inflammatory arthritis), aerobic conditioning, and joint protection education. The referral documents which approaches are recommended based on the patient's condition and limitations.

## Occupational Therapy Referrals

Occupational therapy referrals focus on functional independence and joint protection. The TherapyReferral entity for occupational therapy includes assessment of activities of daily living limitations, recommendations for adaptive equipment (jar openers, button hooks, built-up utensils, splints), joint protection techniques (distributing loads across larger joints, avoiding sustained gripping), and workplace modifications.

Splinting is a specific occupational therapy intervention important in rheumatology. Resting splints for wrists and hands reduce pain during inflammatory flares. Working splints maintain function while protecting joints during activity.

## Functional Goal Tracking

The FunctionalGoal value object defines measurable, time-bound rehabilitation targets. Examples include grip strength measured in kilograms using a dynamometer, timed walking tests (6-minute walk distance), and specific ADL tasks (ability to dress independently, prepare meals, perform work duties). Progress toward goals is documented through TherapyProgressReported events from therapy providers.

## Domain Events

PainPlanCreated is published when a new multimodal plan is established. TherapyProgressReported is published when therapy providers submit progress updates. PainLevelChanged is published when a significant change in pain assessment occurs, potentially triggering reassessment of the overall plan.

## Integration Points

This context consumes DiseaseFlareDetected events from the Autoimmune Disease Context and InjectionAdministered events from the Joint Disease Context. It publishes functional outcome data consumed by the Outcomes Tracking Context. Referrals to external therapy providers use an anti-corruption layer to translate between internal domain models and external referral formats.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Crofford, L.J. "Chronic Pain in Rheumatology: Mechanisms and Principles of Management." Kelley & Firestein's Textbook of Rheumatology, 2021.
- Hurkmans, E., et al. "Dynamic Exercise Programs in RA." Cochrane Database of Systematic Reviews, 2009.
