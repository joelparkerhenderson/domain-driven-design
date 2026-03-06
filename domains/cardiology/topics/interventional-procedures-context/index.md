# Interventional Procedures Context

## Overview

The Interventional Procedures Context manages the planning, execution, and outcomes tracking of catheter-based and surgical cardiac interventions. This bounded context covers cardiac catheterization (diagnostic and therapeutic), percutaneous coronary intervention (PCI) with stenting, transcatheter aortic valve replacement (TAVR), and structural heart interventions. It models the complex procedural workflows from pre-procedural risk assessment through post-procedural follow-up and complication management.

This context consumes clinical referrals and imaging data from upstream contexts and publishes procedural outcomes that inform downstream heart failure management and cardiac rehabilitation.

## Core Concepts

### Cardiac Catheterization

Cardiac catheterization is the foundational invasive diagnostic and therapeutic procedure in interventional cardiology. The context models:

- **Vascular Access**: Access site selection (radial, femoral, brachial), sheath size, and access-related complications (hematoma, pseudoaneurysm, arteriovenous fistula, retroperitoneal hemorrhage).
- **Hemodynamic Assessment**: Intracardiac pressure measurements (right atrial, right ventricular, pulmonary artery, pulmonary capillary wedge, left ventricular, aortic), cardiac output (thermodilution, Fick), and calculated parameters (pulmonary vascular resistance, systemic vascular resistance, valve areas).
- **Coronary Angiography**: Selective coronary artery injection with stenosis assessment using the modified AHA coronary segment model. Each lesion is characterized by location, percent stenosis, length, calcification, bifurcation involvement, and thrombus presence.
- **Left Ventriculography**: Assessment of global and regional left ventricular function, mitral regurgitation severity, and ventricular morphology.

### Percutaneous Coronary Intervention (PCI) and Stenting

PCI models the therapeutic treatment of coronary artery lesions:

- **Lesion Preparation**: Predilation with balloon angioplasty, rotational atherectomy for calcified lesions, intravascular lithotripsy, and cutting balloon use.
- **Stent Implantation**: Drug-eluting stent (DES) selection (diameter, length, platform), deployment parameters (inflation pressure, deployment time), and post-dilation optimization.
- **Intravascular Imaging**: IVUS (intravascular ultrasound) and OCT (optical coherence tomography) for lesion assessment, stent optimization, and complication detection.
- **Physiological Assessment**: FFR (fractional flow reserve) and iFR (instantaneous wave-free ratio) measurements for functional lesion significance determination.
- **SYNTAX Score Calculation**: Quantification of coronary disease complexity from angiographic findings to guide revascularization strategy.

### Transcatheter Aortic Valve Replacement (TAVR)

TAVR models the catheter-based replacement of the aortic valve:

- **Pre-Procedural Planning**: CT-based aortic annulus sizing, access route assessment (transfemoral, transapical, transaortic, subclavian), calcium distribution analysis, and coronary obstruction risk assessment.
- **Heart Team Discussion**: Multidisciplinary evaluation involving interventional cardiologists, cardiac surgeons, and imaging specialists. Models team member roles, discussion findings, and consensus recommendation.
- **Valve Selection**: Prosthesis type (balloon-expandable, self-expanding), size selection based on annular measurements, and manufacturer-specific sizing algorithms.
- **Procedural Execution**: Valve deployment parameters, post-deployment hemodynamics, paravalvular leak assessment, and need for post-dilation or second valve.

### Structural Heart Interventions

- **Left Atrial Appendage Closure**: Device-based stroke prevention for atrial fibrillation patients. Models device selection, implantation parameters, and leak assessment.
- **Mitral Valve Repair**: Transcatheter edge-to-edge repair (MitraClip/PASCAL). Models mitral anatomy assessment, clip placement, and reduction in mitral regurgitation.
- **Patent Foramen Ovale (PFO) and Atrial Septal Defect (ASD) Closure**: Device closure of intracardiac communications. Models defect characterization, device sizing, and closure adequacy.
- **Alcohol Septal Ablation**: Treatment for hypertrophic obstructive cardiomyopathy. Models septal artery identification and gradient reduction.

## Aggregates

- **CatheterizationProcedure**: Root aggregate containing access details, hemodynamics, angiographic findings, and interventions.
- **DeviceImplantation**: Root aggregate for stents, valves, and closure devices with specifications and deployment data.

## Key Domain Events Published

- ProcedureCompleted
- StentImplanted
- ValveReplaced
- ProceduralComplicationOccurred
- HeartTeamRecommendationFinalized

## Integration Points

- **Upstream from Clinical Assessment**: Receives catheterization referrals with risk profiles and clinical findings.
- **Upstream from Diagnostic Imaging**: Receives coronary anatomy, valve assessment, and CT planning data.
- **Partnership with Electrophysiology**: Coordinates hybrid procedures (e.g., LAA closure with ablation).
- **Downstream to Heart Failure Management**: Publishes procedural outcomes affecting HF management.
- **Downstream to Cardiac Rehabilitation**: Publishes procedure completion data for rehabilitation planning.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Lawton et al. 2021 ACC/AHA/SCAI Guideline for Coronary Artery Revascularization. *JACC*, 2022.
- Otto et al. 2020 ACC/AHA Guideline for the Management of Patients with Valvular Heart Disease. *JACC*, 2021.
- ACC NCDR CathPCI Registry Data Dictionary.
