# Periodontal Care Context - Dental Domain

## Overview

The Periodontal Care Context manages the diagnosis, treatment, and ongoing maintenance of periodontal disease. Unlike restorative procedures that address discrete problems, periodontal care follows a chronic disease management model with recurring assessment cycles and maintenance intervals. This context tracks scaling and root planing procedures, monitors pocket depth measurements over time to assess disease progression or improvement, and manages maintenance scheduling based on disease severity and treatment response.

## Core Responsibilities

### Disease Classification

Periodontal disease classification follows the 2017 World Workshop on the Classification of Periodontal and Peri-Implant Diseases and Conditions. The classification system uses two dimensions:

**Staging (I-IV)**: Reflects severity and complexity based on clinical attachment loss, radiographic bone loss, and tooth loss attributable to periodontal disease.
- Stage I: Initial periodontitis with 1-2mm interdental attachment loss.
- Stage II: Moderate periodontitis with 3-4mm attachment loss.
- Stage III: Severe periodontitis with 5mm or greater attachment loss and potential for tooth loss.
- Stage IV: Advanced periodontitis with extensive tooth loss and masticatory dysfunction.

**Grading (A-C)**: Reflects the rate of disease progression and risk factors.
- Grade A: Slow rate of progression with no evidence of bone loss over 5 years.
- Grade B: Moderate rate of progression with bone loss proportional to deposits.
- Grade C: Rapid rate of progression with bone loss exceeding deposit levels.

The context automatically evaluates staging and grading criteria when new probing measurements and radiographic findings are recorded, flagging changes in disease classification.

### Scaling and Root Planing

Scaling and root planing (SRP) is the primary non-surgical treatment for periodontal disease. The context manages SRP procedures by tracking:

- Quadrants treated per session (treatment may be completed in one visit or staged across multiple visits).
- Instrumentation approach (hand scaling, ultrasonic instrumentation, or combination).
- Anesthesia administered for patient comfort during deep scaling.
- Specific teeth and sites requiring focused attention based on probing depths.
- Subgingival irrigation or locally delivered antimicrobial agents when used adjunctively.

The context enforces that all affected quadrants receive treatment within a clinically appropriate timeframe and that re-evaluation is scheduled 4-6 weeks after the final SRP session.

### Pocket Depth Tracking

Longitudinal pocket depth tracking is the core analytical capability of this context. At each assessment, six probing measurements per tooth (mesiobuccal, buccal, distobuccal, mesiolingual, lingual, distolingual) are recorded. The context maintains:

- Complete probing measurement history for trend analysis.
- Comparison between baseline, post-treatment, and maintenance measurements.
- Identification of sites with persistent deep pockets (6mm or greater) that may require surgical intervention.
- Bleeding on probing indicators at each site as a marker of active inflammation.
- Clinical attachment level calculations that account for gingival recession.

The trending data supports clinical decisions about whether the current maintenance interval is adequate or needs adjustment, and whether sites require additional active treatment.

### Maintenance Scheduling

Periodontal maintenance scheduling determines the interval between supportive periodontal therapy appointments based on disease severity, treatment response, and individual risk factors. The context manages:

- Prescribed maintenance intervals (typically 3, 4, or 6 months).
- Interval adjustments based on disease stability or progression at each visit.
- Coordination with the Practice Management Context for appointment scheduling.
- Documentation of maintenance visit procedures (prophylaxis versus periodontal maintenance distinction).

The distinction between prophylaxis (preventive cleaning, CDT code D1110) and periodontal maintenance (therapeutic maintenance after active treatment, CDT code D4910) has both clinical and insurance billing implications that the context manages.

## Aggregate Roots

**Periodontal Record**: Maintains the complete longitudinal history of periodontal assessments, treatments, and disease classification changes for a patient. Enforces that disease classification is updated when measurements indicate a change in status.

## Key Entities

- Periodontal Assessment: A complete set of probing measurements at a point in time.
- Periodontal Treatment Session: A single SRP or maintenance treatment visit.

## Key Value Objects

- Probing Depth: Measurement in millimeters at a specific site.
- Clinical Attachment Level: Derived measurement from probing depth and gingival margin position.
- Bleeding on Probing: Binary indicator at each probing site.
- Periodontal Disease Classification: Stage (I-IV) and Grade (A-C) combination.
- Maintenance Interval: Prescribed months between maintenance appointments.

## Domain Events Produced

- ScalingAndRootPlaningCompleted: Triggers billing and chart notation.
- PeriodontalReevaluationCompleted: Reports treatment response and updated classification.
- PeriodontalMaintenanceScheduled: Configures recall scheduling in Practice Management.
- DiseaseClassificationChanged: Notifies of significant changes in disease status.

## Domain Events Consumed

- PeriodontalProbingRecorded: Receives initial probing data from Clinical Examination.
- TreatmentPlanApproved: Receives authorized periodontal treatment sequences.
- InformedConsentObtained: Verifies consent before treatment initiation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Tonetti, Maurizio S., Henry Greenwell, and Kenneth S. Kornman. Staging and Grading of Periodontitis. Journal of Periodontology, 2018.
- American Academy of Periodontology. Guidelines for Periodontal Therapy. AAP, updated periodically.
