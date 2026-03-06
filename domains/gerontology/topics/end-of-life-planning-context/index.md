# End of Life Planning Context

## Overview

The End of Life Planning Context manages advance directives, palliative care coordination, hospice referral and enrollment, POLST (Physician Orders for Life-Sustaining Treatment) orders, and goals-of-care conversations. End-of-life planning is a sensitive and legally complex aspect of geriatric care that requires careful attention to patient autonomy, family dynamics, cultural considerations, and regulatory requirements.

This context is classified as a supporting subdomain. While deeply important to patient care, the legal frameworks for advance directives and the clinical protocols for palliative and hospice care are well-established. The context supports the core Care Coordination subdomain by ensuring that treatment decisions across all care settings align with the patient's documented values and preferences.

## Ubiquitous Language

Key terms include advance directive, living will, durable power of attorney for healthcare, surrogate decision maker, healthcare proxy, POLST (Physician Orders for Life-Sustaining Treatment), MOLST (Medical Orders for Life-Sustaining Treatment), palliative care, hospice care, comfort care, goals-of-care conversation, do-not-resuscitate (DNR) order, do-not-intubate (DNI) order, code status, full code, comfort measures only, symptom management, prognostic awareness, anticipatory grief, and bereavement support.

## Aggregates

### AdvanceDirective

The primary aggregate for documenting patient treatment preferences. Contains TreatmentPreference value objects (specifying preferences for CPR, mechanical ventilation, artificial nutrition, hospitalization, and antibiotic use), SurrogateDecisionMaker entities (with contact information and scope of authority), and LegalWitness value objects (documenting the witnessing and notarization required for legal validity). Enforces the invariant that the directive meets jurisdictional legal requirements for witness signatures and notarization.

### GoalsOfCareConversation

Models structured discussions between clinicians, patients, and families about care goals and treatment preferences. Contains DiscussionTopic entities (prognosis, treatment options, quality of life priorities), PatientPreference value objects (stated wishes for each decision area), and ConversationOutcome entities (the agreed-upon care direction and any pending decisions). Enforces the invariant that conversation outcomes document both the patient's preferences and the clinical team's recommendations.

### PalliativeCarePlan

Models the palliative care approach for patients with serious illness. Contains SymptomManagementGoal entities (pain control, nausea management, dyspnea relief), PsychosocialSupport entities (counseling, spiritual care, family support), and ComfortCareMeasure value objects. Enforces the invariant that the plan addresses physical, emotional, and spiritual dimensions of suffering.

### HospiceEnrollment

Models the hospice referral and enrollment process. Contains HospiceEligibilityDetermination value objects (prognosis assessment, patient/family agreement), ServicePlan entities (hospice services to be provided), and EnrollmentStatus tracking. Enforces the invariant that hospice enrollment requires documented terminal prognosis and patient or surrogate consent.

## Entities

SurrogateDecisionMaker entity represents the person designated to make healthcare decisions when the patient cannot. It includes relationship, contact information, legal authority documentation, and scope of authority. GoalsOfCareConversation entity records a specific discussion with participants, date, topics, and outcomes. POLSTOrder entity represents a portable medical order translating patient preferences into actionable clinical orders.

## Value Objects

TreatmentPreference captures a specific treatment decision (accept, decline, or undecided) with the date expressed. CodeStatus captures the patient's resuscitation preferences (full code, DNR, DNR/DNI, comfort measures only). PrognosticEstimate captures the clinician's estimated prognosis (months to live, functional trajectory). HospiceEligibilityCriteria captures the clinical criteria supporting hospice referral eligibility.

## Domain Events

AdvanceDirectiveRecorded is published when a patient completes or updates an advance directive. GoalsOfCareConversationCompleted is published when a discussion concludes with documented preferences. HospiceReferralInitiated is published when a hospice referral is made. PalliativeCarePlanEstablished is published when a palliative care approach is formalized. CodeStatusChanged is published when a patient's resuscitation preferences change.

These events are consumed by all other bounded contexts to ensure that care delivery aligns with the patient's documented wishes. The AdvanceDirectiveRecorded event is particularly important because it may constrain treatment decisions in every other context.

## Domain Services

AdvanceCarePlanningEligibilityService determines whether a patient should be offered advance care planning based on age, diagnoses, frailty status, cognitive status, and existing directive status. GoalsOfCareAlignmentService evaluates whether a patient's current care plan aligns with their documented goals of care, identifying discrepancies that require clinical attention.

## Clinical Workflow

End-of-life planning is ideally an ongoing process that begins early and evolves with the patient's clinical trajectory. The AdvanceCarePlanningEligibilityService identifies patients who should be offered advance care planning conversations. A goals-of-care conversation is conducted with the patient (if capable) and family, exploring values, preferences, and priorities.

The conversation outcomes are documented in the GoalsOfCareConversation aggregate. If the patient wishes to formalize their preferences, an AdvanceDirective aggregate is created with the specific treatment preferences and surrogate decision-maker designation. When appropriate, a POLST order translates preferences into actionable medical orders.

As the patient's condition progresses, palliative care may be introduced alongside curative treatment. When curative treatment is no longer desired or effective, hospice referral is initiated. The hospice enrollment process verifies eligibility, obtains consent, and establishes the hospice service plan.

Throughout this process, the context publishes events that inform other bounded contexts. A change in code status affects emergency treatment protocols. A hospice enrollment shifts medication management toward comfort-focused prescribing and adjusts care coordination to the hospice model.

## Integration Points

This context consumes FrailtyStatusChanged, CognitiveDeclineDetected, and FunctionalDeclineDetected events from other contexts as triggers for advance care planning conversations. It publishes AdvanceDirectiveRecorded and GoalsOfCareConversationCompleted events that constrain treatment decisions across all contexts.

The context integrates with legal document management systems through an anti-corruption layer and with hospice agency referral systems through published language interfaces.

## Regulatory Considerations

Advance directive requirements vary by state jurisdiction, including witness requirements, notarization, and surrogate designation rules. POLST programs operate under state-specific regulations. Hospice enrollment must comply with Medicare Hospice Benefit requirements, including the certification of terminal illness with a prognosis of six months or less.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Sudore, R.L. et al. (2017). Defining Advance Care Planning for Adults: A Consensus Definition from a Multidisciplinary Delphi Panel. Journal of Pain and Symptom Management.
- National POLST. (2019). POLST Legislative Comparison Guide.
