# Diagnostic Imaging Context

## Overview

The Diagnostic Imaging Context manages the full lifecycle of cardiac imaging studies, from order receipt through image acquisition, measurement, interpretation, and report finalization. This bounded context covers four major cardiac imaging modalities: echocardiography, cardiac computed tomography (CT), cardiac magnetic resonance imaging (MRI), and nuclear cardiology. Each modality has its own acquisition protocols, measurement sets, and reporting standards, but all share common workflow patterns within this context.

This context operates as both a consumer of clinical referrals from the Clinical Assessment Context and a supplier of structured imaging results to multiple downstream contexts.

## Core Concepts

### Echocardiography

Echocardiography is the most frequently performed cardiac imaging modality. The context models:

- **Transthoracic Echocardiography (TTE)**: Standard non-invasive ultrasound examination with standardized views (parasternal long axis, parasternal short axis, apical 4-chamber, apical 2-chamber, subcostal). Measurements include chamber dimensions, volumes, ejection fraction, wall motion assessment, valve function, and Doppler hemodynamics.
- **Transesophageal Echocardiography (TEE)**: Semi-invasive examination providing higher-resolution images for valvular assessment, intracardiac thrombus detection, and procedural guidance. Models sedation requirements and procedural consent.
- **Stress Echocardiography**: Combined exercise or pharmacological stress with echocardiographic imaging. Coordinates with the Clinical Assessment Context for stress protocol execution.
- **3D Echocardiography**: Volumetric imaging for precise chamber quantification and valve assessment. Models additional measurement sets specific to 3D analysis.

### Cardiac CT

- **Coronary CT Angiography (CCTA)**: Non-invasive assessment of coronary artery anatomy for stenosis detection. Models coronary artery segment evaluation using the modified AHA 18-segment model, stenosis grading, plaque characterization (calcified, non-calcified, mixed), and CT-derived fractional flow reserve (CT-FFR).
- **Coronary Artery Calcium Scoring**: Quantification of coronary calcification using the Agatston scoring method. Models calcium score values with percentile rankings by age and sex.
- **Cardiac CT for Structural Assessment**: CT imaging for TAVR planning (aortic annulus sizing, access route assessment), left atrial appendage evaluation, and congenital heart disease assessment.

### Cardiac MRI

- **Functional Assessment**: Cine MRI for precise chamber volumes, ejection fraction, wall motion, and myocardial mass. Considered the reference standard for ventricular quantification.
- **Tissue Characterization**: Late gadolinium enhancement (LGE) for scar and fibrosis detection. T1 and T2 mapping for diffuse fibrosis and edema quantification. Models myocardial segment-level tissue characterization.
- **Stress Perfusion MRI**: Pharmacological stress with first-pass perfusion imaging for ischemia detection.
- **Flow Assessment**: Phase-contrast MRI for flow quantification across valves and great vessels.

### Nuclear Cardiology

- **SPECT Myocardial Perfusion Imaging**: Radiotracer-based perfusion assessment at rest and stress for ischemia detection and quantification. Models perfusion defect severity (mild, moderate, severe), extent (number of segments), and reversibility (fixed vs. reversible).
- **PET Myocardial Perfusion**: Higher-resolution perfusion imaging with quantitative myocardial blood flow measurement in mL/g/min.
- **PET Viability Assessment**: FDG-PET for myocardial viability determination in ischemic cardiomyopathy.

## Aggregates

- **ImagingStudy**: Root aggregate containing study metadata, acquisition protocol, structured measurements, and interpretation report.
- **ImagingOrder**: Root aggregate containing clinical indication, priority, scheduling requirements, and order status.

## Key Value Objects

- EjectionFraction, ChamberDimension, ValveGradient, WallMotionScore, CalciumScore, PerfusionDefect, LateGadoliniumEnhancement, CoronaryStenosis (CT-based).

## Key Domain Events Published

- ImagingStudyCompleted
- CriticalImagingFindingDetected
- EjectionFractionChanged
- SevereValvularDiseaseIdentified

## Integration Points

- **Upstream from Clinical Assessment**: Receives imaging referral events with clinical indications.
- **Published Language to all consumers**: Publishes structured imaging results using standardized measurement formats (ASE, SCCT, SCMR guidelines).
- **Downstream to Heart Failure Management**: Provides serial ejection fraction and chamber measurements.
- **Downstream to Interventional Procedures**: Provides coronary anatomy and valve assessment data for procedural planning.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Lang et al. Recommendations for Cardiac Chamber Quantification by Echocardiography in Adults. *JASE*, 2015.
- SCCT Guidelines on the Use of Coronary CT Angiography. *Journal of Cardiovascular Computed Tomography*, 2022.
- Kramer et al. Standardized Cardiovascular Magnetic Resonance Imaging Protocols. *JCMR*, 2020.
