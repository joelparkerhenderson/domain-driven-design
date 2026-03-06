# Bounded Contexts in the Gerontology Domain

## Overview

Bounded contexts are the central pattern in strategic Domain-Driven Design. A bounded context defines a logical boundary within which a particular domain model is consistent and complete. In the gerontology domain, bounded contexts reflect the natural clinical and organizational divisions in geriatric care, where different teams, workflows, and vocabularies govern different aspects of an older adult's care.

Evans (2003) emphasizes that a model is only meaningful within a specific context. The term "assessment" means something different in geriatric screening than in cognitive testing. By establishing bounded contexts, the gerontology domain ensures that these distinctions are explicit, preventing model corruption and reducing confusion among development teams and clinical stakeholders.

## Geriatric Assessment Context

This context encompasses comprehensive geriatric assessment (CGA), frailty screening, fall risk evaluation, and multidimensional health status determination. The CGA is the foundational process in geriatrics, evaluating medical, functional, psychological, and social dimensions of an older adult's health. Key entities include the GeriatricAssessment aggregate, FrailtyScreening, FallRiskEvaluation, and NutritionalAssessment. The context produces assessment outcomes consumed by other contexts to guide care planning.

## Care Coordination Context

This context manages the interdisciplinary care team, care transitions between settings, caregiver support programs, and community resource linkage. Care coordination is critical in gerontology because older adults frequently receive care from multiple providers across fragmented healthcare systems. Key entities include CarePlan, CareTeam, CareTransition, CaregiverSupportPlan, and CommunityResourceReferral. The context consumes assessment results and produces coordinated care plans.

## Cognitive Health Context

This context addresses dementia screening, cognitive testing using standardized instruments such as MMSE and MoCA, cognitive decline tracking over time, and memory care planning. Cognitive health is a distinct area of geriatric care with specialized vocabulary, clinical workflows, and care models. Key entities include CognitiveScreening, DementiaStaging, MemoryCarePlan, and CognitiveTrendAnalysis. The context interacts with the Geriatric Assessment Context for initial screening and with Care Coordination for care plan integration.

## Functional Independence Context

This context covers ADL and IADL assessment, mobility aid prescription, home modification planning, and assistive technology management. Functional independence is a core concern in gerontology because the ability to perform daily activities directly impacts quality of life, care needs, and living arrangements. Key entities include FunctionalAssessment, MobilityAidPrescription, HomeModificationPlan, and AssistiveTechnologyRecommendation. The context receives input from geriatric assessments and informs care coordination decisions.

## Medication Management Context

This context handles polypharmacy review, application of the Beers criteria to identify potentially inappropriate medications, deprescribing protocols, medication reconciliation during care transitions, and medication adherence monitoring. Older adults are disproportionately affected by adverse drug events due to age-related pharmacokinetic changes and the prevalence of polypharmacy. Key entities include MedicationRegimen, PolypharmacyReview, DeprescribingPlan, and MedicationAdherenceRecord.

## End of Life Planning Context

This context manages advance directives, palliative care coordination, hospice referral and enrollment, POLST orders, and goals-of-care conversations. End-of-life planning requires sensitivity to patient values, family dynamics, and legal requirements. Key entities include AdvanceDirective, PalliativeCarePlan, HospiceEnrollment, GoalsOfCareConversation, and POLSTOrder. The context integrates with Care Coordination for care plan alignment and with regulatory compliance for legal documentation requirements.

## Context Boundaries

Each bounded context maintains its own model of the patient. In the Geriatric Assessment Context, the patient is modeled with assessment-specific attributes such as frailty score, fall risk level, and nutritional status. In the Medication Management Context, the patient is modeled with medication-specific attributes such as current regimen, renal function, and allergy profile. These models may share a patient identifier but differ in structure and behavior.

Boundaries are enforced through published interfaces and domain events. When a geriatric assessment identifies cognitive concerns, a domain event is published that the Cognitive Health Context subscribes to, initiating its own detailed screening process without coupling to the assessment model.

## Team Alignment

Each bounded context is owned by a team with appropriate domain expertise. The Geriatric Assessment Context is owned by clinical informatics specialists working closely with geriatricians. The Medication Management Context is owned by pharmacy informatics specialists. This alignment ensures that the models within each context reflect genuine clinical knowledge rather than developer assumptions.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Rubenstein, L.Z. et al. (1991). Impacts of Geriatric Evaluation and Management Programs on Defined Outcomes. JAGS.
