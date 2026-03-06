# Domain Services - Dental Domain

## Overview

Domain services are stateless operations that implement domain logic which does not naturally belong to a single entity or value object. In the dental domain, domain services handle operations that span multiple aggregates, coordinate complex clinical workflows, or implement business rules that cross entity boundaries. A domain service for treatment cost estimation, for example, must consider procedure fees, insurance benefit calculations, and patient financial history, spanning multiple aggregates.

## Domain Service Design Principles

Domain services in the dental domain are named using verbs that reflect the ubiquitous language. They are stateless, meaning they do not maintain any internal state between invocations. They coordinate between aggregates and value objects but do not bypass aggregate invariants. Domain services are distinct from application services (which orchestrate use cases) and infrastructure services (which handle technical concerns).

## Domain Services by Bounded Context

### Clinical Examination Context

**CariesRiskAssessmentService**
Evaluates a patient's overall caries risk based on examination findings, medical history, dietary factors, and fluoride exposure. This service operates across the Patient Clinical Record aggregate and produces a risk classification (low, moderate, high) that informs recall interval recommendations and preventive treatment suggestions.

Input: Patient clinical record, medical history factors, behavioral risk indicators.
Output: Caries risk classification with contributing factor analysis.

**PeriodontalScreeningService**
Performs an initial periodontal screening assessment using probing measurements to determine whether the patient requires comprehensive periodontal evaluation. This service applies threshold rules (probing depths exceeding 3mm, bleeding on probing patterns) to examination findings.

Input: Probing measurements from the clinical record.
Output: Screening result indicating healthy, gingivitis, or suspected periodontitis with referral recommendation.

### Treatment Planning Context

**TreatmentSequencingService**
Determines the clinically appropriate order for procedures in a treatment plan based on dependency rules. Extractions must precede prosthetic replacements. Endodontic treatment must precede crown placement on a pulpally involved tooth. Emergency procedures take priority over elective ones. This service enforces sequencing invariants that span multiple treatment items.

Input: List of proposed treatment items with procedure codes and tooth designations.
Output: Sequenced treatment plan with phase assignments and dependency links.

**CostEstimationService**
Calculates the estimated cost for a treatment plan by applying fee schedules, insurance benefit rules, deductible accumulations, and annual maximum remaining amounts. This service coordinates between treatment plan items and insurance coverage data to produce patient-facing cost estimates.

Input: Treatment plan items, applicable fee schedule, patient insurance coverage details.
Output: Itemized cost estimate with insurance portion, patient responsibility, and confidence indicators.

**CasePresentationService**
Assembles the clinical findings, treatment options, and cost information into a structured case presentation format. This service coordinates data from the examination record, treatment plan, and cost estimates to create a unified presentation for patient communication.

Input: Examination findings, treatment plan, cost estimates.
Output: Structured case presentation with diagnosis summary, treatment options, and financial overview.

### Restorative Services Context

**MaterialSelectionService**
Recommends appropriate restorative materials based on tooth location, cavity size, occlusal load, aesthetic requirements, and patient allergies. This service applies clinical guidelines to suggest material options (amalgam, composite, ceramic) with trade-off analysis for each option.

Input: Tooth number, surfaces involved, cavity classification, patient material sensitivities.
Output: Ranked material recommendations with clinical rationale.

**ProcedureStagingService**
Determines the multi-visit staging sequence for indirect restorations. Crown procedures require preparation, impression (or digital scan), temporary placement, and cementation visits. This service calculates appropriate timing intervals between stages and identifies prerequisites.

Input: Procedure type, tooth condition, patient schedule constraints.
Output: Staged visit sequence with recommended timing intervals.

### Orthodontic Management Context

**MalocclusionAssessmentService**
Evaluates the patient's occlusal relationship using Angle's classification, overjet and overbite measurements, and dental and skeletal analysis to produce a comprehensive malocclusion assessment. This service synthesizes clinical measurements and cephalometric data.

Input: Clinical occlusal measurements, cephalometric analysis values.
Output: Malocclusion classification with severity assessment and treatment complexity estimate.

**TreatmentProgressEvaluationService**
Compares current tooth positions against the predicted treatment trajectory to evaluate whether orthodontic treatment is progressing as expected. This service identifies teeth that are tracking behind predicted movement and suggests adjustment modifications.

Input: Current progress measurements, predicted treatment trajectory, elapsed treatment time.
Output: Progress evaluation with tracking assessment and adjustment recommendations.

### Periodontal Care Context

**DiseaseClassificationService**
Applies the current periodontal disease staging and grading criteria to probing measurements, radiographic bone loss, and clinical attachment levels. This service determines the appropriate stage (I-IV) and grade (A-C) based on the 2017 World Workshop classification system.

Input: Probing depth measurements, clinical attachment levels, radiographic bone loss percentages.
Output: Periodontal disease classification with stage, grade, and extent.

**MaintenanceIntervalService**
Determines the appropriate periodontal maintenance interval based on disease classification, treatment response, and risk factors. Patients with well-controlled disease may be maintained at 6-month intervals, while those with aggressive disease may require 3-month intervals.

Input: Current disease classification, treatment response history, systemic risk factors.
Output: Recommended maintenance interval with clinical justification.

### Practice Management Context

**InsuranceEligibilityService**
Verifies patient insurance eligibility and retrieves current benefit details including remaining annual maximums, deductible status, and coverage percentages by procedure category. This service coordinates between the patient account and external insurance verification systems.

Input: Patient insurance policy details, verification request parameters.
Output: Eligibility confirmation with benefit summary.

**RecallSchedulingService**
Determines appropriate recall scheduling based on clinical risk assessment, patient preferences, and provider availability. This service coordinates between clinical risk factors and scheduling constraints to recommend optimal recall dates.

Input: Patient clinical risk assessment, scheduling preferences, provider availability.
Output: Recommended recall date with appointment parameters.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on domain services.
