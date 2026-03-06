# Cardiology Standards

## Overview

Cardiology standards are clinical, technical, and procedural guidelines that inform domain model design across all bounded contexts. In Domain-Driven Design, standards serve as published languages and shared domain knowledge that shape value object definitions, aggregate invariants, and domain service logic. The cardiology domain is governed by an extensive body of professional society guidelines, coding standards, and data exchange specifications that must be reflected in the domain model.

## Clinical Practice Guidelines

### ACC/AHA Guidelines

The American College of Cardiology (ACC) and American Heart Association (AHA) jointly publish clinical practice guidelines that are the primary authoritative source for cardiology domain logic:

- **Heart Failure Management**: 2022 AHA/ACC/HFSA Guideline for the Management of Heart Failure. Defines heart failure classification (HFrEF, HFmrEF, HFpEF), GDMT pillars, titration targets, and escalation criteria. Directly shapes the Heart Failure Management Context's aggregate invariants and domain service logic.

- **Chest Pain Evaluation**: 2021 ACC/AHA Guideline for the Evaluation and Diagnosis of Chest Pain. Defines clinical pathways for acute and stable chest pain evaluation. Shapes the Clinical Assessment Context's risk stratification and referral logic.

- **Coronary Revascularization**: 2021 ACC/AHA/SCAI Guideline for Coronary Artery Revascularization. Defines indications for PCI vs. CABG, appropriate use criteria, and Heart Team decision-making. Shapes the Interventional Procedures Context's revascularization strategy domain service.

- **Valvular Heart Disease**: 2020 ACC/AHA Guideline for the Management of Patients with Valvular Heart Disease. Defines severity grading criteria, intervention thresholds, and surveillance intervals. Informs both Diagnostic Imaging and Interventional Procedures contexts.

- **Atrial Fibrillation**: 2023 ACC/AHA/ACCP/HRS Guideline for Diagnosis and Management of Atrial Fibrillation. Defines classification, stroke risk assessment (CHA2DS2-VASc), rate and rhythm control strategies, and ablation indications. Directly shapes Electrophysiology Context models.

- **Ventricular Arrhythmias**: 2017 AHA/ACC/HRS Guideline for Management of Patients with Ventricular Arrhythmias and Prevention of Sudden Cardiac Death. Defines ICD implantation criteria and VT management algorithms.

- **Bradycardia and Conduction Delay**: 2018 ACC/AHA/HRS Guideline on the Evaluation and Management of Bradycardia. Defines pacemaker implantation indications and device selection criteria.

- **Prevention**: 2019 ACC/AHA Guideline on Primary Prevention of Cardiovascular Disease. Defines risk factor targets and lifestyle modification recommendations informing the Cardiac Rehabilitation Context.

### Imaging Society Guidelines

Professional imaging societies publish measurement standards that define value object structures:

- **ASE (American Society of Echocardiography)**: Chamber quantification guidelines define normal ranges, measurement methods, and severity classifications for all echocardiographic parameters. These directly shape EjectionFraction, ChamberDimension, and ValveGradient value objects.

- **SCCT (Society of Cardiovascular Computed Tomography)**: Guidelines for coronary CTA interpretation, calcium scoring methodology (Agatston), and CT-based procedural planning standards.

- **SCMR (Society for Cardiovascular Magnetic Resonance)**: Standardized CMR protocols, measurement guidelines, and tissue characterization criteria.

- **ASNC (American Society of Nuclear Cardiology)**: Nuclear perfusion imaging acquisition protocols, interpretation guidelines, and quantitative analysis standards.

## Coding and Classification Standards

### ICD-10-CM Diagnosis Codes

The International Classification of Diseases, 10th Revision, Clinical Modification provides diagnosis coding that the domain model must support:

- I20-I25: Ischemic heart diseases (angina, acute MI, chronic ischemic heart disease).
- I26-I28: Pulmonary heart disease and diseases of pulmonary circulation.
- I30-I52: Other forms of heart disease (pericarditis, endocarditis, cardiomyopathy, heart failure, arrhythmias).
- I60-I69: Cerebrovascular diseases (relevant for stroke risk in AF patients).

### CPT Procedure Codes

Current Procedural Terminology codes for cardiology procedures inform the billing ACL:

- 93303-93355: Echocardiography codes.
- 93451-93572: Cardiac catheterization and coronary intervention codes.
- 93600-93662: Electrophysiology procedure codes.
- 93797-93798: Cardiac rehabilitation session codes.
- 75571-75574: Cardiac CT codes.
- 75557-75565: Cardiac MRI codes.

### SNOMED CT

Systematized Nomenclature of Medicine - Clinical Terms provides clinical terminology for structured data exchange. SNOMED CT codes map to cardiology domain concepts for interoperability with external systems.

## Data Exchange Standards

### HL7 FHIR

Fast Healthcare Interoperability Resources (FHIR) provides the data exchange framework for integration with external systems. Relevant FHIR resources include Observation (for vital signs and biomarkers), DiagnosticReport (for imaging reports), Procedure (for interventions), and Condition (for diagnoses).

### DICOM

Digital Imaging and Communications in Medicine standard governs cardiac image storage and exchange. The Diagnostic Imaging Context's integration with picture archiving and communication systems (PACS) follows DICOM standards.

## Registry Standards

### ACC NCDR

The ACC National Cardiovascular Data Registry defines data elements for quality reporting:

- **CathPCI Registry**: Standardized data collection for cardiac catheterization and PCI outcomes.
- **ICD Registry**: Device implantation and outcomes data.
- **STS/ACC TVT Registry**: Transcatheter valve therapy outcomes.
- **LAAO Registry**: Left atrial appendage occlusion outcomes.

These registry data definitions influence entity attributes and aggregate structures within the Interventional Procedures and Electrophysiology contexts.

## Standards Governance in the Domain Model

Standards are not static. Guidelines are updated every 3-5 years, coding systems receive annual updates, and new data exchange specifications emerge. The domain model must accommodate standards evolution through:

- Versioned guideline criteria in domain services.
- Configurable reference ranges in value objects.
- Extensible coding system mappings in anti-corruption layers.
- Guideline update tracking in domain metadata.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- ACC/AHA Clinical Practice Guideline Manual of Procedures, 2022.
- HL7 International. FHIR R4 Specification. https://hl7.org/fhir/.
- DICOM Standards Committee. Digital Imaging and Communications in Medicine.
