# Clinical Examination Context - Dental Domain

## Overview

The Clinical Examination Context is the foundational bounded context in the dental domain. It owns all diagnostic activities performed during patient visits and serves as the authoritative source for the patient's current oral health status. This context encompasses oral examination procedures, radiographic imaging, dental charting, caries detection, and periodontal probing. All downstream clinical decisions, treatment plans, and administrative processes depend on the accuracy and completeness of data captured within this context.

## Core Responsibilities

### Oral Examination

The oral examination process records findings from visual inspection, tactile exploration, and functional assessment of the oral cavity. Examinations are classified by type: comprehensive (new patient or significant interval), periodic (recall visit), limited (focused problem), and emergency (acute complaint). Each examination type has specific documentation requirements and expected findings coverage.

The examination records findings at multiple levels: tooth-level conditions (caries, fractures, mobility), soft tissue findings (lesions, inflammation), occlusal relationships, and temporomandibular joint assessment. Each finding is attributed to the examining provider and timestamped for the clinical record.

### Radiographic Imaging

Radiographic imaging management covers the ordering, acquisition, and interpretation of dental X-rays. The context manages imaging protocols that determine which views are appropriate based on patient age, risk factors, and clinical indications. Standard series include full-mouth (typically 18-20 periapical and bitewing images), bitewing-only (4 images for caries detection), and panoramic imaging.

Radiographic interpretation records are linked to the corresponding images and document findings such as interproximal caries, periapical pathology, bone levels, and incidental findings. Interpretation is always attributed to the interpreting provider.

### Dental Charting

Dental charting maintains a graphical record of every tooth's current status, including existing restorations with materials, missing teeth with reason (extracted, congenitally absent, unerupted), and current pathology. The chart uses the universal numbering system (1-32 for permanent, A-T for primary teeth) and standard surface notation (mesial, occlusal/incisal, distal, buccal/facial, lingual/palatal).

The chart is a living document updated after every clinical event: new findings during examination, completed restorations reported from the Restorative Services Context, and orthodontic or periodontal status changes reported from their respective contexts.

### Caries Detection

Caries detection systematically identifies and classifies tooth decay. Detection methods include visual inspection, tactile exploration with an explorer, radiographic evaluation, and adjunctive technologies (laser fluorescence, transillumination). Each detected lesion is recorded with its tooth location, surfaces involved, classification (incipient, moderate, advanced, severe), and detection method.

### Periodontal Probing

Periodontal probing measures the depth of the gingival sulcus or periodontal pocket at six sites per tooth using a calibrated periodontal probe. Measurements are recorded in millimeters along with bleeding on probing indicators. Full-mouth probing data provides the basis for periodontal disease classification and is shared with the Periodontal Care Context through the shared kernel.

## Aggregate Roots

**Patient Clinical Record**: The primary aggregate maintaining the dental chart, examination history, and current clinical status for a single patient.

**Radiographic Series**: Groups related radiographic images from a single imaging session with their interpretations.

## Key Entities

- Patient: The person receiving dental care, identified by a practice-assigned number.
- Tooth: An individual tooth identified by universal number, tracking condition history.
- Examination: A single clinical examination event with type, date, and provider.
- Radiograph: An individual radiographic image with type, projection, and findings.

## Key Value Objects

- Probing Depth: Measurement in millimeters at a specific site.
- Tooth Number: Universal numbering system identifier.
- Tooth Surface: Surface designation for findings and procedures.
- Caries Classification: Severity and location classification of a lesion.

## Domain Events Produced

- ExaminationCompleted: Triggers treatment planning for identified conditions.
- CariesDetected: Notifies treatment planning of restorative needs.
- PeriodontalProbingRecorded: Updates the periodontal record via shared kernel.
- RadiographInterpreted: Provides imaging findings for treatment decisions.

## Domain Events Consumed

- RestorationCompleted: Updates the dental chart with new restorations.
- EndodonticTreatmentCompleted: Updates tooth vitality status on the chart.
- OrthodonticProgressRecorded: Updates orthodontic status notations.
- ScalingAndRootPlaningCompleted: Notes periodontal treatment on the chart.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Dental Association. CDT: Current Dental Terminology. ADA, updated annually.
- American Dental Association. Dental Radiographic Examinations: Recommendations for Patient Selection and Limiting Radiation Exposure. ADA/FDA, 2012.
