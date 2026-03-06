# Heart Failure Management Context

## Overview

The Heart Failure Management Context focuses on the longitudinal care of patients with heart failure, from initial classification through guideline-directed medical therapy (GDMT) optimization, device therapy integration, and escalation to advanced therapies. This bounded context is classified as a core subdomain because it embodies the most complex clinical reasoning in cardiology: balancing multiple medication classes, titrating against physiological parameters, and making high-stakes decisions about mechanical circulatory support and transplantation.

This context maintains a shared kernel with the Clinical Assessment Context for vital signs and biomarkers, consumes imaging data from the Diagnostic Imaging Context, integrates device data from the Electrophysiology Context, and supplies therapy status to the Cardiac Rehabilitation Context.

## Core Concepts

### Heart Failure Classification

The context models two complementary classification systems:

**ACC/AHA Staging (A through D)**:
- Stage A: At risk for heart failure (hypertension, diabetes, CAD, cardiotoxin exposure) without structural disease or symptoms.
- Stage B: Structural heart disease (reduced EF, LVH, valvular disease, prior MI) without current or prior symptoms.
- Stage C: Structural heart disease with current or prior heart failure symptoms.
- Stage D: Advanced heart failure requiring specialized interventions (refractory to standard GDMT).

A critical invariant: ACC/AHA stages can only advance (A to B to C to D), never regress, because they represent cumulative disease progression.

**NYHA Functional Classification (I through IV)**:
- Class I: No limitation of physical activity.
- Class II: Slight limitation; comfortable at rest, ordinary activity causes symptoms.
- Class III: Marked limitation; comfortable at rest, less than ordinary activity causes symptoms.
- Class IV: Unable to carry on any physical activity without discomfort; symptoms at rest.

Unlike ACC/AHA staging, NYHA class can improve or worsen with treatment, reflecting current functional status.

**Heart Failure Phenotyping**:
- HFrEF: LVEF 40% or less. Primary indication for four-pillar GDMT.
- HFmrEF (mildly reduced): LVEF 41-49%. Emerging evidence for GDMT benefit.
- HFpEF: LVEF 50% or greater. Different pathophysiology; SGLT2 inhibitors and diuretics are primary therapies.
- HFimpEF (improved): Previously reduced EF that has improved to >40%. GDMT should be continued.

### Guideline-Directed Medical Therapy (GDMT)

GDMT optimization is the central domain logic of this bounded context. The four pillars of HFrEF therapy are:

**Pillar 1 - Beta-Blockers**: Carvedilol, metoprolol succinate, or bisoprolol. Target doses are drug-specific. Titration is limited by heart rate (<60 bpm), blood pressure (<90 mmHg systolic), and symptoms (fatigue, dizziness). The domain model tracks current dose, target dose, percentage of target achieved, and reasons for dose limitations.

**Pillar 2 - RAAS Inhibitors (ACEi/ARB/ARNI)**: Sacubitril/valsartan (ARNI) preferred over ACEi/ARB. Titration limited by blood pressure, potassium (>5.5 mEq/L), and creatinine (>30% rise). A 36-hour washout period is required when switching from ACEi to ARNI.

**Pillar 3 - Mineralocorticoid Receptor Antagonists (MRA)**: Spironolactone or eplerenone. Requires monitoring of potassium and renal function. Contraindicated if GFR <30 mL/min or potassium >5.0 mEq/L.

**Pillar 4 - SGLT2 Inhibitors**: Dapagliflozin or empagliflozin. Indicated for both HFrEF and HFpEF regardless of diabetes status. Fewer titration barriers than other pillars.

The GDMTOptimizationService evaluates current regimen status, identifies next titration steps, checks safety parameters, and generates a prioritized optimization plan.

### Volume Status Monitoring

Heart failure management requires tracking fluid balance:

- Daily weight trends with alert thresholds for acute gain (>2 lbs/day or >5 lbs/week).
- Diuretic dosing (loop diuretics, thiazide diuretics) with dose-response tracking.
- Renal function monitoring (creatinine, BUN, electrolytes) during diuresis.
- Signs and symptoms of congestion (dyspnea, orthopnea, edema, JVD) modeled as clinical status value objects.

### Mechanical Circulatory Support

For Stage D patients failing optimal GDMT:

- **INTERMACS Profiling**: Profiles 1-7 classifying advanced HF severity, guiding MCS timing.
- **Left Ventricular Assist Device (LVAD)**: Bridge to transplant, bridge to decision, or destination therapy. Models device type, implant parameters, pump speed, flow, power, and pulsatility index.
- **Intra-Aortic Balloon Pump (IABP)**: Temporary hemodynamic support. Models trigger timing, augmentation ratio, and weaning protocol.
- **Transplant Evaluation**: Models listing criteria, UNOS status, panel reactive antibody levels, and waitlist management.

## Aggregates

- **HeartFailureProfile**: Root aggregate for phenotype, staging, NYHA class, EF history, and comorbidities.
- **GDMTRegimen**: Root aggregate for current medications, target doses, titration history, and contraindications.

## Key Domain Events Published

- HeartFailureClassificationChanged
- GDMTOptimizationAchieved
- MechanicalSupportEscalation
- VolumeOverloadDetected
- EjectionFractionRecovered

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Heidenreich et al. 2022 AHA/ACC/HFSA Guideline for the Management of Heart Failure. *JACC*, 2022.
- Maddox et al. 2024 ACC Expert Consensus Decision Pathway for Treatment of Heart Failure. *JACC*, 2024.
- Mehra et al. International Society for Heart and Lung Transplantation Listing Criteria, 2016.
