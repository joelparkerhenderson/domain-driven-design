# Care Coordination Context

## Overview

The Care Coordination Context manages the interdisciplinary team-based approach to geriatric care, care transitions between settings, caregiver support programs, and linkage to community resources. Care coordination is a core subdomain in the gerontology domain because effective coordination across fragmented healthcare delivery systems is the primary determinant of outcomes for older adults with complex, multimorbid conditions.

Older adults typically receive care from multiple providers across various settings: primary care clinics, specialist offices, hospitals, skilled nursing facilities, home health agencies, and community programs. Without systematic coordination, care becomes fragmented, duplicated, or contradictory. This context models the workflows, team structures, and communication patterns that unify care delivery.

## Ubiquitous Language

Key terms include interdisciplinary care team, care plan, care goal, care activity, care transition, discharge planning, care conference, care manager, caregiver burden, caregiver support plan, respite care, community resource referral, patient-centered medical home, care episode, and care intensity level. These terms have precise definitions within this context.

## Aggregates

### CarePlan

The central aggregate root. Contains CareGoal entities (with target outcomes, responsible providers, and status tracking), CareActivity entities (with schedules, assignments, and completion records), and ProviderAssignment value objects. Enforces the invariant that every activity links to a goal and every goal has an assigned provider. Supports versioning to track plan evolution over time.

### CareTransition

Models patient movement between care settings. Contains TransferSummary, ReceivingProviderNotification, and MedicationReconciliationRecord entities. Enforces the invariant that transitions cannot be completed without medication reconciliation and a transfer summary. Publishes CareTransitionInitiated and CareTransitionCompleted events.

### CaregiverSupportPlan

Models the assessment and support of informal caregivers. Contains CaregiverBurdenAssessment value objects, RespiteCareSchedule entities, and SupportServiceReferral entities. Enforces the invariant that high-burden caregivers receive referrals for support services.

## Entities

CareTeamMember represents a participant in the interdisciplinary team with role, credentials, contact information, and involvement period. CareGoal represents a specific clinical or functional objective with target, timeline, and status. CommunityResourceReferral tracks referrals to external services with status and outcome. CareConference records team meetings with attendees, topics, decisions, and action items.

## Value Objects

CareGoalStatus captures status (active, achieved, modified, discontinued) with date and reason. CareSettingLocation captures the type of care setting, address, and contact information. CareIntensityLevel captures the level of care coordination required (standard, enhanced, intensive) based on patient complexity. TransitionRiskScore captures the assessed risk of adverse events during a care transition.

## Domain Events

CarePlanCreated is published when a new care plan is established. CareTransitionInitiated is published when a patient begins moving between settings. CaregiverBurdenAssessed is published when caregiver assessment reveals high strain. CareGoalAchieved is published when a care plan goal reaches its target outcome. CareConferenceScheduled is published to notify team members of upcoming team meetings.

## Domain Services

CareTeamAssemblyService recommends team composition based on assessment results and patient needs. CareTransitionRiskService evaluates risk associated with proposed transitions and recommends mitigation strategies. These services span multiple aggregates and apply coordination best practices.

## Clinical Workflow

Care coordination begins when assessment results trigger care plan creation. The care manager assembles the interdisciplinary team, convenes a care conference, and develops a comprehensive care plan with goals and activities. The care plan is executed across team members and care settings, with regular reassessment and plan updates.

When a care transition occurs, the care manager initiates the transition process, ensures medication reconciliation, prepares transfer summaries, and notifies receiving providers. Post-transition follow-up confirms that the patient's care plan is successfully continued in the new setting.

Caregiver support is an ongoing concern. Caregiver burden is assessed regularly, and high-burden caregivers are connected with respite care, support groups, and community services to prevent caregiver burnout and its negative effects on patient care.

## Integration Points

This context consumes AssessmentCompleted events from the Geriatric Assessment Context to trigger care planning. It consumes CognitiveDeclineDetected events from Cognitive Health to adjust care plan complexity. It publishes CarePlanCreated and CareTransitionInitiated events consumed by Medication Management, Functional Independence, and End of Life Planning.

The context integrates with insurance systems through an anti-corruption layer for service authorization and with community service directories through an open host service pattern for resource matching.

## Regulatory Considerations

Care coordination must comply with CMS care transition requirements, including the Transitional Care Management (TCM) documentation standards. Caregiver support activities must align with the RAISE Family Caregivers Act and state-specific caregiver support mandates.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Naylor, M.D. et al. (2011). The Importance of Transitional Care in Achieving Health Reform. Health Affairs.
- Coleman, E.A. & Boult, C. (2003). Improving the Quality of Transitional Care for Persons with Complex Care Needs. JAGS.
