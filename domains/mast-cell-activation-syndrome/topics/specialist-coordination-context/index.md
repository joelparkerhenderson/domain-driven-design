# Specialist Coordination Context for Mast Cell Activation Syndrome

## Overview

The Specialist Coordination Context is a supporting bounded context that manages the multi-provider nature of MCAS care. Patients with MCAS typically require ongoing involvement from multiple specialists including allergists, immunologists, gastroenterologists, dermatologists, cardiologists, neurologists, and primary care providers. This context coordinates referrals, shared care plans, and inter-provider communication to ensure coherent, non-contradictory management across the care team.

The ubiquitous language within this context centers on care coordination concepts such as care team, shared care plan, referral, consultation, care team member role, and provider communication. These terms describe the organizational structures and workflows needed to manage complex multi-specialty MCAS care.

## Key Aggregates

### Care Team

The Care Team is the primary aggregate root. It represents the group of healthcare providers actively involved in a patient's MCAS management. The aggregate contains Care Team Member entities, each with a provider identifier, specialty, role (lead, consultant, co-manager), and involvement period.

The aggregate enforces several invariants. Each care team must have a designated lead provider, typically an allergist or immunologist with MCAS expertise. No duplicate specialty assignments are allowed unless explicitly justified with different roles. Changes to team composition are tracked with effective dates to maintain a history of care team evolution.

### Referral

The Referral aggregate manages the lifecycle of a specialist consultation request. It progresses through states: requested, accepted, scheduled, completed, and reported. The aggregate contains the referring provider, target specialty, clinical reason, urgency level, and the consultation outcome when available.

The aggregate enforces the invariant that a referral must have a referring provider, a target specialty, and a clinical reason before submission. Urgency levels follow a defined classification: routine, urgent, and emergent.

### Shared Care Plan

The Shared Care Plan aggregate represents the coordinated treatment strategy agreed upon by the care team. It contains treatment goals, medication recommendations from each specialist, monitoring schedules, and escalation protocols. The aggregate maintains version history to track how the management approach evolves over time.

The care plan aggregate enforces the invariant that all included treatment recommendations must be attributable to a specific care team member. Conflicting recommendations are flagged but not automatically resolved, as clinical judgment is required.

## Value Objects

The Provider Specialty value object classifies a care team member's medical specialty. For MCAS, relevant specialties include allergy and immunology, gastroenterology, dermatology, cardiology, neurology, hematology, endocrinology, and primary care.

The Care Team Member Role value object defines the provider's function within the team: lead provider (primary responsibility for MCAS management), co-manager (shared management responsibility for a specific aspect), or consultant (episodic involvement for specific concerns).

The Referral Urgency value object classifies the priority of a referral request. It includes a clinical justification that explains the urgency level in domain-specific terms.

## Domain Events

ReferralRequested is published when a new specialist referral is initiated. The event includes the target specialty, clinical reason, and urgency level. ReferralCompleted is published when a consultation is finished and results are available.

CarePlanUpdated is published when the shared care plan is modified by any care team member. This event is consumed by all clinical contexts to ensure that treatment decisions across the system align with the current coordinated plan.

CareTeamChanged is published when a provider is added to or removed from the care team. This event informs other contexts about changes in the provider roster.

## Domain Services

The CareTeamAssemblyService recommends specialist involvement based on the patient's symptom profile, diagnostic results, and organ system involvement. A patient with predominant gastrointestinal symptoms would receive a recommendation for gastroenterology involvement, while one with cardiovascular manifestations would be directed toward cardiology.

The CarePlanReconciliationService harmonizes treatment recommendations from multiple specialists. It identifies conflicts such as medication interactions across prescribers, contradictory dietary recommendations, or overlapping monitoring schedules. The service presents conflicts for clinical resolution rather than resolving them automatically.

## Specialist Roles in MCAS Management

### Allergist/Immunologist

Typically serves as the lead provider for MCAS management. Responsible for diagnostic confirmation, overall treatment strategy, and medication regimen oversight. Manages the core antihistamine and mast cell stabilizer medications.

### Gastroenterologist

Manages gastrointestinal manifestations of MCAS. Evaluates for eosinophilic conditions, mast cell infiltration of the GI tract, and other GI complications. Provides dietary management guidance and manages GI-targeted medications.

### Dermatologist

Evaluates and manages skin manifestations including chronic urticaria, angioedema, flushing, and dermatographism. May perform skin biopsies to assess mast cell density in affected tissues.

### Cardiologist

Evaluates cardiovascular symptoms including tachycardia syndromes, blood pressure dysregulation, and chest pain. Differentiates MCAS-related cardiovascular symptoms from primary cardiac conditions.

### Primary Care Provider

Coordinates overall health maintenance, manages comorbid conditions, and serves as the point of contact for acute issues between specialist visits. Monitors for medication interactions across all prescribers.

## Communication Patterns

The context supports structured communication between providers through the shared care plan, referral reports, and care team notifications. All provider-to-provider communication is documented within the context to maintain a comprehensive coordination history.

## Integration Points

The Specialist Coordination Context consumes events from all other clinical contexts to maintain an up-to-date view of the patient's management status. It integrates with external EHR systems through an anti-corruption layer for provider directory information and scheduling data. The context publishes care plan updates that inform treatment decisions across the system.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Afrin, L.B. et al. "Diagnosis of Mast Cell Activation Syndrome: A Global Consensus-2." Diagnosis, 2020.
