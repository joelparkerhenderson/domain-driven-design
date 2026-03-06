# Inflammatory Disease Context

## Overview

The Inflammatory Disease Context manages the longitudinal care of patients with
inflammatory bowel disease (IBD), encompassing both Crohn's disease and ulcerative
colitis. This context handles disease classification, activity scoring, biologic
and immunomodulator therapy management, flare detection and treatment, and remission
maintenance. IBD management requires sophisticated longitudinal tracking because the
disease is chronic, relapsing, and requires continuous therapy optimization.

## Scope and Responsibilities

This context is responsible for:

- Classifying IBD type and extent using the Montreal Classification system.
- Tracking disease activity over time using validated scoring instruments.
- Managing biologic therapy selection, initiation, monitoring, and switching.
- Managing conventional immunomodulator therapy alongside or independent of biologics.
- Detecting disease flares through symptom monitoring and biomarker surveillance.
- Implementing treat-to-target strategies that aim for mucosal healing.
- Coordinating with the Endoscopic Procedures Context for surveillance colonoscopy.
- Tracking disease complications such as strictures, fistulae, and dysplasia.

## Core Aggregates

### IBDDiagnosis

The foundational aggregate that represents a patient's inflammatory bowel disease
diagnosis over time. Contains the disease type (Crohn's or ulcerative colitis),
Montreal Classification (age at diagnosis, location, behavior for Crohn's; extent
for UC), disease activity score history, complication history, and current disease
state.

Key invariants: the Montreal Classification must be internally consistent (e.g.,
Crohn's location must be one of L1-L4, behavior B1-B3 with optional perianal modifier).
Disease activity scores must use the instrument appropriate for the disease type
(Harvey-Bradshaw Index for Crohn's, Mayo Score for UC).

### TherapyRegimen

Manages the treatment plan for a patient's IBD. Contains current and historical
therapy entries including agent name, mechanism of action, dosing schedule,
administration route, therapeutic drug monitoring results, and clinical response
assessments. The regimen tracks therapy switches and the clinical rationale for
each change.

Key invariant: active biologic therapies must have associated monitoring schedules
that comply with prescribing information requirements. Therapy changes must have
documented rationale.

### FlareEpisode

Represents a discrete episode of disease exacerbation. Contains flare onset date,
trigger identification (if known), severity classification (mild, moderate, severe),
symptom documentation, inflammatory marker values, treatment actions taken, and
resolution status.

Key invariant: flare severity must be classified using standardized criteria, and
treatment escalation must follow a logical stepwise approach documented in the
treatment actions.

## Key Entities

- ActivityScoreEntry: a dated disease activity assessment with score value and
  component breakdown.
- TherapyEntry: a specific medication within the regimen with dosing and monitoring data.
- ComplicationRecord: documentation of IBD complications such as stricture, fistula,
  abscess, or dysplasia.

## Key Value Objects

- MontrealClassification: IBD classification by age, location, and behavior.
- HarveyBradshawIndex: Crohn's disease activity score (0-20+).
- MayoScore: ulcerative colitis activity score (0-12).
- FecalCalprotectin: inflammatory biomarker value with interpretation threshold.
- CReactiveProtein: systemic inflammatory marker value.
- TherapeuticDrugLevel: biologic trough level with target range comparison.
- EndoscopicSubscore: endoscopic component of disease activity assessment.

## Domain Events

- FlareDetected: triggers urgent clinical response and therapy reassessment.
- TherapyAdjusted: notifies pharmacy integration and monitoring coordination.
- RemissionAchieved: triggers surveillance scheduling and maintenance optimization.
- ComplicationIdentified: initiates surgical consultation or intervention planning.
- SurveillanceColonoscopyDue: coordinates with the Endoscopic Procedures Context.

## Clinical Workflows

### New Diagnosis Workflow

When IBD is initially diagnosed (typically via endoscopy with histologic confirmation),
the IBDDiagnosis aggregate is created with initial Montreal Classification, baseline
disease activity score, and initial therapy plan. The TherapyRegimen aggregate is
created with the selected induction therapy.

### Treat-to-Target Workflow

The treat-to-target approach establishes specific therapeutic goals (clinical remission,
biochemical remission, endoscopic remission) and systematically adjusts therapy to
achieve them. The DiseaseActivityScoringService evaluates progress at each assessment.
If targets are not met, the TherapyOptimizationService suggests adjustments.

### Flare Management Workflow

When symptoms or biomarkers indicate a flare, the FlareEpisode aggregate is created.
Severity is classified, and treatment escalation follows a structured approach:
optimize current therapy, consider dose escalation, add combination therapy, or
switch mechanism of action. The FlareDetected event alerts the clinical team.

### Surveillance Workflow

Patients with longstanding IBD require regular surveillance colonoscopy for dysplasia
detection. The context tracks surveillance intervals and raises
SurveillanceColonoscopyDue events consumed by the Endoscopic Procedures Context.

## Integration Points

- Upstream: receives endoscopic findings and pathology results from the Endoscopic
  Procedures Context.
- Upstream: receives lab results (CRP, fecal calprotectin) from Clinical Assessment.
- Downstream: triggers surveillance scheduling in Endoscopic Procedures Context.
- Downstream: communicates dietary requirements to Nutrition Management Context.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Turner, D. et al. (2021). "STRIDE-II: an update on the IOIBD treat-to-target." Gastroenterology.
- Silverberg, M.S. et al. (2005). "The Montreal classification of IBD." Canadian Journal of Gastroenterology.
