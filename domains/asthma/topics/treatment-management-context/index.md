# Treatment Management Context - Asthma Domain

## Overview

The Treatment Management Context is responsible for managing therapeutic interventions, treatment planning, and therapy optimization for asthma patients. This bounded context owns the models for stepwise therapy according to GINA guidelines, biologic agent therapy, allergen immunotherapy, and inhaler technique assessment. It is one of two core subdomains in the asthma management domain.

Treatment management translates clinical assessment findings into therapeutic action. It implements the evidence-based stepwise approach that is the cornerstone of modern asthma care, where treatment intensity is escalated or de-escalated based on the patient's current control level, risk profile, and treatment response.

## Key Concepts

**Stepwise Therapy** follows the GINA five-step approach. Step 1 provides as-needed low-dose ICS-formoterol or as-needed SABA with ICS taken whenever SABA is used. Step 2 adds daily low-dose ICS as a controller. Step 3 escalates to low-dose ICS-LABA combination. Step 4 increases to medium-dose ICS-LABA. Step 5 adds high-dose ICS-LABA with consideration of add-on therapies including biologic agents, tiotropium, or low-dose oral corticosteroids. The principle of minimal effective therapy guides the goal of achieving good control at the lowest step possible.

**Biologic Agents** are targeted monoclonal antibody therapies for severe asthma uncontrolled on Step 4-5 conventional therapy. Omalizumab targets IgE for allergic asthma. Mepolizumab and benralizumab target the IL-5 pathway for eosinophilic asthma. Dupilumab targets IL-4 and IL-13 for eosinophilic and Type 2 asthma. Tezepelumab targets TSLP across multiple phenotypes. Selection depends on the patient's phenotype, endotype, biomarker profile, and comorbidities.

**Allergen Immunotherapy** gradually desensitizes the immune system to specific allergens. Subcutaneous immunotherapy (SCIT) involves escalating injections over months followed by maintenance dosing. Sublingual immunotherapy (SLIT) provides allergen tablets or drops for home use. Immunotherapy is indicated for patients with allergic asthma and demonstrated allergen sensitivity, particularly when allergen avoidance and pharmacotherapy provide insufficient control.

**Inhaler Technique Assessment** evaluates the patient's ability to correctly use prescribed inhalation devices. Poor inhaler technique is a leading cause of treatment failure. Different device types (metered-dose inhalers, dry powder inhalers, soft mist inhalers, nebulizers) require different techniques. Assessment includes device-specific checklist scoring and corrective instruction.

## Aggregates

### TreatmentPlan

The central aggregate. Contains the current GINA step, medication assignments, step change history, and treatment goals. Enforces that only one treatment plan is active per patient, that LABA is never prescribed without concurrent ICS, and that step changes are documented with clinical rationale.

### BiologicTherapy

Manages the lifecycle of a biologic agent therapy from eligibility assessment through initiation, periodic response assessment, and potential discontinuation. Enforces eligibility criteria verification and scheduled response assessments at 4-month intervals.

### ImmunotherapyProtocol

Tracks allergen immunotherapy schedules including dose escalation phases, maintenance dosing, and adverse reaction monitoring. Enforces safe dose escalation intervals and mandatory observation periods after injections.

## Entities

- **MedicationAssignment:** A specific medication within a treatment plan with drug, dose, frequency, and device.
- **StepChangeRecord:** An immutable record of a treatment step change with rationale and approving clinician.
- **ResponseAssessment:** A periodic evaluation of biologic therapy response (good, partial, non-response).
- **InhalerTechniqueScore:** A scored checklist assessment of patient inhaler technique for a specific device.

## Value Objects

- **TherapyStep:** GINA step number (1-5) with preferred and alternative therapy definitions.
- **EligibilityCriteria:** Biomarker thresholds and clinical requirements for biologic agent eligibility.
- **DosingSchedule:** Medication frequency, dose amount, units, and administration route.
- **TreatmentGoal:** Target control level and lung function goals for the treatment plan.

## Domain Events Published

- **TreatmentPlanSteppedUp:** Triggers medication prescription changes and action plan updates.
- **TreatmentPlanSteppedDown:** Triggers medication discontinuation and action plan revision.
- **BiologicTherapyInitiated:** Triggers specialty pharmacy prescription and outcome tracking initiation.
- **BiologicResponseAssessed:** Informs outcome tracking and may trigger therapy continuation or switch.
- **InhalerTechniqueAssessed:** Records technique scores for adherence analysis.

## Domain Events Consumed

- **AssessmentFinalized (from Clinical Assessment):** Triggers evaluation of whether current therapy step is appropriate.
- **SeverityReclassified (from Clinical Assessment):** Triggers treatment plan review for potential step change.
- **AdherenceAlertRaised (from Medication Management):** Step-up evaluations must consider whether non-adherence rather than therapy inadequacy explains poor control.
- **OutcomeTrendAlertRaised (from Outcomes Tracking):** Declining trends trigger proactive treatment plan review.
- **ExacerbationRecorded (from Outcomes Tracking):** Severe or repeated exacerbations trigger step-up evaluation.

## Domain Services

- **StepwiseTherapyService:** Evaluates step-up and step-down criteria, verifies modifiable factors, and recommends specific therapies.
- **BiologicEligibilityService:** Assesses patient eligibility for biologic agents and recommends agent selection based on immunological profile.

## Integration Points

- **Downstream from Clinical Assessment:** Receives assessment results that drive treatment decisions. Customer-supplier relationship.
- **Upstream to Medication Management:** Treatment plan changes drive prescription generation. Customer-supplier relationship.
- **Upstream to Action Planning:** Treatment instructions are published in standardized format. Published language relationship.
- **Bidirectional with Outcomes Tracking:** Receives outcome trend alerts; provides treatment context for outcome analysis.

## Business Rules

1. LABA medications must never be prescribed as monotherapy without concurrent ICS (FDA black box warning compliance).
2. Step-up decisions must first verify modifiable factors: medication adherence above 80%, correct inhaler technique, trigger avoidance compliance.
3. Step-down requires at least 3 months of well-controlled asthma with no exacerbations.
4. Biologic therapy initiation requires documented failure of optimized Step 4 conventional therapy.
5. Biologic response assessment must occur at 4-month intervals; non-response requires therapy switch or discontinuation.
6. Allergen immunotherapy requires confirmed allergen sensitivity via skin prick test or specific IgE measurement.
7. Treatment plan modifications require clinician sign-off and documented clinical rationale.
8. Shared decision-making must be documented for therapy choices with meaningful alternatives.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023. Chapters on pharmacological management.
- National Asthma Education and Prevention Program (NAEPP). Expert Panel Report 4: Guidelines for the Diagnosis and Management of Asthma, 2020.
