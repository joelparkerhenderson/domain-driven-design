# Survivorship Care Context

## Overview

The Survivorship Care Context manages the long-term care of cancer patients after completing active treatment. This bounded context addresses the transition from active treatment to post-treatment surveillance, monitoring for cancer recurrence, managing late and long-term treatment effects, coordinating psychosocial support, and generating comprehensive survivorship care plans. The survivorship phase may last for decades, and the domain model must support this extended timeframe while adapting to evolving clinical guidelines and changing patient needs.

## Ubiquitous Language

Within this context, "survivorship" encompasses the period from diagnosis onward, though this context primarily focuses on the post-treatment phase. A "survivorship care plan" is a document summarizing the treatment received, recommended follow-up schedule, potential late effects, and wellness strategies. "Late effects" are treatment-related complications that emerge months or years after treatment completion (distinct from acute side effects that occur during treatment). "Surveillance" is the systematic monitoring for cancer recurrence through scheduled examinations and tests.

## Aggregate Roots

### Survivorship Care Plan

The Survivorship Care Plan is the primary aggregate root. It contains the treatment summary (all cancer-directed treatments received across all modalities), the recommended surveillance schedule (examinations and tests at specified intervals), the late effects monitoring plan (potential complications to watch for based on treatments received), wellness recommendations (exercise, nutrition, mental health), and care coordination information (primary care provider, specialists to maintain contact with).

The aggregate enforces several invariants. The surveillance schedule must be consistent with current guidelines for the patient's cancer type, stage, and treatments received. The late effects monitoring plan must address all known risks associated with the administered therapies. The treatment summary must accurately reflect the treatments documented in the treatment modality contexts.

### Psychosocial Support Record

The Psychosocial Support Record aggregate tracks the psychosocial care provided to cancer survivors. It contains screening assessments for distress, anxiety, depression, and fear of recurrence, referrals to psychosocial support services (counseling, support groups, social work), and follow-up on referral outcomes. The aggregate ensures that distress screening is performed at guideline-recommended intervals and that identified needs are addressed with appropriate referrals.

## Key Entities

**Follow-Up Visit**: An entity representing a scheduled surveillance encounter. Each visit has a unique identifier, a scheduled date, the surveillance tests to be performed (imaging studies, laboratory tests, physical examination), the actual examination findings, the assessment of recurrence status, and any changes to the surveillance plan based on findings.

**Late Effect**: An entity tracking a specific long-term or delayed treatment side effect. Each late effect has a unique identifier, the causative treatment (specific drug, radiation site, surgical procedure), the date of onset, the current severity, the management approach, and the monitoring frequency. Late effects may be chronic and require indefinite monitoring.

**Wellness Goal**: An entity representing a specific wellness objective for the survivor, such as an exercise program, nutritional goal, or smoking cessation plan. Tracks the goal definition, the current status, progress milestones, and supporting resources provided.

**Care Transition Record**: An entity documenting the transition of care responsibilities from the oncology team to the primary care provider. Captures the transition date, the responsibilities transferred, the responsibilities retained by the oncology team, and the communication plan between providers.

## Key Value Objects

**Performance Status**: The patient's functional capacity measured on the ECOG or Karnofsky scale.

**Follow-Up Interval**: A recommended surveillance interval specifying the test type, the starting point (months from treatment completion), and the frequency (for example, every six months for years one through three, then annually).

**Distress Score**: A standardized measure of psychosocial distress, typically using the NCCN Distress Thermometer (0-10 scale) with associated problem checklist categories.

**Treatment Summary Entry**: A record of a specific treatment received, including the modality, the specific treatment (drug names and cumulative doses, radiation site and total dose, surgical procedure), and the treatment dates.

**Recurrence Risk Level**: A categorization of the patient's estimated risk of cancer recurrence (low, intermediate, high) based on cancer type, stage, treatment response, and prognostic factors.

## Domain Events

**SurvivorshipCarePlanGenerated**: A new or updated survivorship care plan has been created.
**SurveillanceTestDue**: A scheduled follow-up test or examination is approaching its due date.
**SurveillanceTestCompleted**: A follow-up test has been performed and results are available.
**RecurrenceSuspected**: Surveillance findings suggest possible cancer recurrence, triggering diagnostic workup.
**LateEffectIdentified**: A new late treatment effect has been diagnosed in a cancer survivor.
**LateEffectResolved**: A previously identified late effect has resolved or is no longer active.
**DistressScreeningCompleted**: A psychosocial distress screening assessment has been performed.
**CareTransitionCompleted**: The formal transition of care responsibilities to the primary care provider has been documented.
**GuidelineUpdateApplied**: The survivorship care plan has been updated to reflect revised clinical guidelines.

## Domain Services

**FollowUpScheduleGenerationService**: Creates a personalized follow-up surveillance schedule based on the cancer type, stage, treatments received, and current clinical guidelines from ASCO and NCCN.

**LateEffectRiskAssessmentService**: Evaluates a survivor's risk for specific late treatment effects based on the therapies received, administered doses, and individual risk factors. Produces a risk profile informing the monitoring plan.

## Integration Points

This context is downstream of all treatment modality contexts (Chemotherapy Management, Radiation Therapy, Surgical Oncology). It subscribes to treatment completion events from each context to assemble the comprehensive treatment summary. The anti-corruption layer translates modality-specific treatment records into the survivorship context's treatment summary format.

This context interfaces with the Diagnosis and Staging Context when surveillance findings suggest possible recurrence. A RecurrenceSuspected event triggers diagnostic workup in the Diagnosis and Staging Context, potentially restarting the oncology care cycle.

This context also integrates with primary care systems through a published language of survivorship care plan documents that follow ASCO or Commission on Cancer (CoC) templates, enabling primary care providers to understand their role in the survivor's ongoing care.

## Long-Term Considerations

The survivorship care domain model must accommodate extended timeframes. Follow-up schedules may span decades. Late effects may emerge years after treatment. Guidelines evolve over time, and care plans must be updatable when new evidence changes recommendations. The model must support versioning of care plans and tracking of which guideline versions informed each plan iteration.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Society of Clinical Oncology. ASCO Survivorship Care Guidelines. https://www.asco.org/practice-patients/guidelines/survivorship
- Commission on Cancer. Cancer Program Standards: Ensuring Patient-Centered Care. American College of Surgeons, 2020.
