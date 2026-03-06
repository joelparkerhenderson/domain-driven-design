# Surgical Planning Context

## Overview

The Surgical Planning Context is the bounded context responsible for the pre-operative
specification of orthopaedic surgical procedures. It encompasses pre-operative
assessment, surgical approach selection, implant selection and templating, operative
risk evaluation, and scheduling coordination. This context transforms diagnostic
findings from Clinical Assessment into a complete, approved operative plan.

## Scope

This context covers:

- Pre-operative medical assessment and fitness for surgery evaluation.
- Surgical approach selection based on anatomy, pathology, and surgeon expertise.
- Implant selection and digital templating against patient imaging.
- Operative risk scoring using validated tools.
- Surgical plan documentation and approval workflow.
- Coordination with anaesthesia and theatre scheduling.

This context does not cover: the clinical assessment that precedes planning, the
intraoperative execution, post-operative implant tracking, or rehabilitation.

## Ubiquitous Language

- Surgical Plan: The complete pre-operative specification for a procedure.
- Surgical Approach: The anatomical route to the operative site.
- Implant Template: Projected implant size and position on imaging.
- Pre-Operative Assessment: Medical evaluation confirming fitness for surgery.
- Operative Risk Score: Quantified surgical complication probability.
- Plan Approval: Formal sign-off by the operating surgeon.
- Templating: The process of sizing implants against patient anatomy on images.

## Aggregate Root

**SurgicalPlan** is the aggregate root. It ensures internal consistency between the
selected approach, chosen implants, templating results, risk assessment, and approval
status. A plan cannot be approved without a completed pre-operative assessment.
Selected implants must be compatible with the planned approach.

## Key Entities

- SurgicalPlan: Transitions through draft, reviewed, approved, and executed states.
- Surgeon: Operating surgeon with credentials and implant preferences.

## Key Value Objects

- SurgicalApproachType: Named approach (posterior, anterior, lateral, etc.).
- ImplantSize: Size designation, material, and catalog number.
- TemplatingResult: Projected component size and positioning measurements.
- RiskScore: Calculated risk value with contributing factors and scoring system used.
- BMI: Body mass index calculated from height and weight.
- ASAGrade: American Society of Anesthesiologists physical status classification.

## Domain Events Published

- SurgicalPlanApproved: Emitted when the surgeon approves the plan.
- ImplantSelectionFinalized: Emitted when implant choices are locked for procurement.

## Domain Events Consumed

- PatientAssessed: From Clinical Assessment, triggering plan creation.
- DiagnosisConfirmed: From Clinical Assessment, providing diagnostic context.
- FractureClassified: From Fracture Management, for operative fracture planning.

## Invariants

- A plan cannot be approved without a completed pre-operative assessment.
- Selected implants must be compatible with each other and the planned approach.
- Templating measurements must correspond to the selected implant specifications.
- Risk assessment must be completed before plan approval.
- Only the designated operating surgeon can approve a plan.
- An approved plan cannot be modified; changes require a new plan version.

## Domain Services

- ImplantCompatibilityService: Validates implant component compatibility.
- TemplatingService: Calculates optimal implant sizing from imaging measurements.
- OperativeRiskService: Computes risk scores from patient and procedure factors.

## Clinical Workflow

1. PatientAssessed event triggers creation of a draft Surgical Plan.
2. Surgeon selects the surgical approach based on anatomy and pathology.
3. Digital templating determines implant sizing against patient imaging.
4. Implant components are selected and validated for compatibility.
5. Pre-operative assessment confirms patient fitness for surgery.
6. Operative risk is calculated and documented.
7. Surgeon reviews and approves the plan.
8. SurgicalPlanApproved event is published.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- The British Orthopaedic Association. BOAST Standards. https://www.boa.ac.uk
