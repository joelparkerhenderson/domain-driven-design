# Electrophysiology Context

## Overview

The Electrophysiology Context addresses the diagnosis and treatment of cardiac rhythm disorders. This bounded context encompasses arrhythmia classification and management, catheter ablation procedures, pacemaker and implantable cardioverter-defibrillator (ICD) implantation, and electrophysiology (EP) studies. It models the specialized workflows of cardiac electrophysiologists who focus on the electrical conduction system of the heart.

This context maintains a partnership relationship with the Interventional Procedures Context for hybrid procedures and acts as a supplier to the Heart Failure Management Context for cardiac resynchronization therapy (CRT) data.

## Core Concepts

### Arrhythmia Management

The context models the classification, diagnosis, and treatment of cardiac arrhythmias:

- **Supraventricular Arrhythmias**: Atrial fibrillation (paroxysmal, persistent, long-standing persistent, permanent), atrial flutter (typical, atypical), supraventricular tachycardia (AVNRT, AVRT, AT), and inappropriate sinus tachycardia. Each arrhythmia type carries specific classification attributes, management algorithms, and treatment options.
- **Ventricular Arrhythmias**: Ventricular tachycardia (monomorphic, polymorphic, sustained, non-sustained), ventricular fibrillation, and premature ventricular complexes (PVC burden quantification). Includes substrate characterization (ischemic vs. non-ischemic) and risk stratification for sudden cardiac death.
- **Bradyarrhythmias**: Sinus node dysfunction (sinus bradycardia, sinus arrest, sinoatrial block), atrioventricular block (first, second type I, second type II, third degree), and bundle branch blocks. Models indication assessment for pacing therapy.
- **Rhythm Monitoring**: Holter monitoring (24-48 hour), extended continuous monitoring (7-30 days), implantable loop recorder data, and event monitor correlation. Models rhythm detection algorithms and clinically significant episode identification.

### Ablation Procedures

Catheter ablation uses energy delivery to destroy abnormal cardiac tissue causing arrhythmias:

- **Atrial Fibrillation Ablation**: Pulmonary vein isolation (PVI) strategy with wide-area circumferential ablation. Models left atrial anatomy, pulmonary vein ostial geometry, ablation lesion sets, entrance and exit block verification, and adjunctive strategies (posterior wall isolation, cavotricuspid isthmus ablation, mitral isthmus line).
- **Energy Modalities**: Radiofrequency ablation (point-by-point or contact force-guided), cryoablation (balloon-based), and pulsed field ablation (PFA). Each modality has distinct energy delivery parameters, lesion characteristics, and safety profiles modeled as value objects.
- **Ventricular Tachycardia Ablation**: Substrate-based approach using voltage mapping (scar identification), pace mapping, activation mapping, and entrainment mapping. Models three-dimensional electroanatomic maps with ablation lesion locations.
- **SVT Ablation**: Targeted ablation of specific arrhythmia circuits (slow pathway modification for AVNRT, accessory pathway ablation for AVRT, focal ablation for AT).
- **Ablation Endpoints**: Arrhythmia non-inducibility, bidirectional conduction block, elimination of abnormal signals. Modeled as domain-specific success criteria value objects.

### Pacemaker and ICD Implantation

Cardiac implantable electronic device (CIED) management encompasses:

- **Device Selection**: Single-chamber, dual-chamber, biventricular (CRT-P, CRT-D), subcutaneous ICD (S-ICD), leadless pacemaker, and His-bundle or left bundle branch area pacing. Selection criteria based on indication, patient anatomy, and lifestyle requirements.
- **Implantation Procedure**: Generator pocket creation, lead placement (right atrial appendage, right ventricular apex or septum, coronary sinus branch for LV lead), threshold testing, and lead fixation (active vs. passive).
- **Device Programming**: Mode selection (DDD, VVI, AAI, CRT), rate parameters (lower rate, upper tracking rate, sensor rate), AV delay optimization, detection and therapy zones for ICDs, and anti-tachycardia pacing (ATP) algorithms.
- **Device Follow-Up**: Remote monitoring data, in-office interrogation results, battery longevity projections, lead integrity surveillance, and therapy delivery history.

### EP Studies

Electrophysiology studies are invasive diagnostic procedures:

- **Baseline Measurements**: Sinus node recovery time, AH interval, HV interval, Wenckebach cycle length, and AV block cycle length. Modeled as conduction interval value objects with normal range references.
- **Pacing Protocols**: Programmed stimulation with decremental pacing and extrastimuli delivery to assess arrhythmia inducibility. Models stimulation sites, coupling intervals, and induced rhythm responses.
- **Mapping Systems**: Three-dimensional electroanatomic mapping (CARTO, EnSite) producing voltage maps, activation maps, and propagation maps. Models geometric chamber reconstructions with electrical data overlay.

## Aggregates

- **EPStudy**: Root aggregate containing intracardiac recordings, pacing maneuvers, mapping data, and inducibility results.
- **AblationProcedure**: Root aggregate containing ablation target, energy delivery parameters, lesion locations, and endpoint achievement.
- **CIEDImplantation**: Root aggregate containing device model, lead placement data, thresholds, and initial programming.

## Key Domain Events Published

- ArrhythmiaDetected
- DeviceImplanted
- AblationCompleted
- DeviceAlertTriggered
- LeadIntegrityCompromised

## Integration Points

- **Partnership with Interventional Procedures**: Hybrid procedures combining structural intervention and ablation.
- **Downstream to Heart Failure Management**: CRT device data for optimization and response assessment.
- **Upstream from Clinical Assessment**: Receives arrhythmia evaluation referrals.
- **ACL with Device Manufacturers**: Translates proprietary device data into unified domain model.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Joglar et al. 2023 ACC/AHA/ACCP/HRS Guideline for Diagnosis and Management of Atrial Fibrillation. *Circulation*, 2024.
- Al-Khatib et al. 2017 AHA/ACC/HRS Guideline for Management of Patients with Ventricular Arrhythmias. *JACC*, 2018.
- Kusumoto et al. 2018 ACC/AHA/HRS Guideline on Evaluation and Management of Bradycardia and Cardiac Conduction Delay. *JACC*, 2019.
