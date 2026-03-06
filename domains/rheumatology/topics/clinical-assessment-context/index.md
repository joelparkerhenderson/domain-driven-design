# Clinical Assessment Context

## Overview

The Clinical Assessment Context manages the structured evaluation of patients presenting with musculoskeletal and autoimmune symptoms. It is the primary data-producing context in the rheumatology domain, generating the clinical findings that downstream contexts consume for disease classification, treatment decisions, and outcome tracking. This context encompasses joint examinations, serologic testing, inflammatory marker measurement, and composite disease activity score calculation.

## Context Boundaries

The Clinical Assessment Context owns the definition of what constitutes a complete rheumatologic evaluation. It determines which joints are examined, which laboratory tests are ordered, which scoring instruments are applied, and how results are interpreted. It does not make treatment decisions, classify diseases, or manage therapies. These responsibilities belong to the Autoimmune Disease, Joint Disease, and Biologic Therapy contexts respectively.

The boundary is drawn to keep assessment logic independent of treatment logic. A clinician performing an assessment follows standardized examination protocols regardless of what treatment the patient is receiving. This separation allows assessment protocols to evolve independently from treatment guidelines.

## Aggregate: PatientAssessment

The PatientAssessment aggregate root is the central object in this context. It represents a single clinical evaluation episode and maintains consistency across all findings recorded during that visit. The aggregate contains JointExamination entities, SerologyResult value objects, InflammatoryMarker value objects, and calculated DiseaseActivityScore value objects.

The PatientAssessment enforces the invariant that composite scores cannot be calculated until all required component measurements are present. A DAS28-ESR score requires tender joint count from 28 specified joints, swollen joint count from 28 specified joints, an ESR value, and a patient global assessment on a 0-100 visual analog scale. If any component is missing, the score cannot be computed.

## Joint Examination Workflow

Joint examination follows a standardized protocol. The 28-joint count assesses bilateral proximal interphalangeal joints (10), metacarpophalangeal joints (10), wrists (2), elbows (2), shoulders (2), and knees (2). Each joint is assessed for tenderness (pain on pressure) and swelling (soft tissue enlargement). The 66/68-joint count extends examination to feet, ankles, hips, and additional hand joints.

The JointExamination entity records individual joint findings and computes aggregate counts. Tenderness and swelling are recorded as binary (present/absent) per joint. The entity validates that all joints in the selected scheme have been assessed before marking the examination as complete.

## Serology Panel

Serologic testing in rheumatology includes rheumatoid factor (RF), which is measured as IgM-RF titer with positivity typically defined as above the upper limit of normal for the assay. Anti-cyclic citrullinated peptide (anti-CCP) antibodies are measured in units with positivity thresholds specific to the assay manufacturer. Antinuclear antibody (ANA) testing reports titer (1:40, 1:80, 1:160, 1:320, 1:640) and pattern (homogeneous, speckled, nucleolar, centromere, peripheral). Complement levels (C3, C4, CH50) are measured to assess complement consumption in lupus and vasculitis.

The SerologyResult value object captures these results with sufficient detail for clinical interpretation. The SerologyInterpretationService applies assay-specific reference ranges and clinical interpretation rules.

## Disease Activity Scoring

The ClinicalScoringService calculates composite disease activity indices. DAS28-ESR is computed as 0.56 times the square root of TJC28 plus 0.28 times the square root of SJC28 plus 0.70 times the natural logarithm of ESR plus 0.014 times patient global VAS. The resulting score is interpreted as remission (below 2.6), low disease activity (2.6 to 3.2), moderate disease activity (3.2 to 5.1), or high disease activity (above 5.1).

CDAI (Clinical Disease Activity Index) sums TJC28, SJC28, patient global (0-10 cm), and evaluator global (0-10 cm). SDAI (Simplified Disease Activity Index) adds CRP in mg/dL to the CDAI formula. RAPID3 uses only patient-reported measures (function, pain, patient global) for rapid assessment.

## Domain Events

The AssessmentCompleted event is published when a clinician finalizes the assessment. It carries the complete assessment data including scores and interpretation. The SerologyResultReceived event is published when laboratory results become available. The CriticalLabResultDetected event is published when results indicate potential safety concerns.

## Integration Points

The Clinical Assessment Context publishes assessment results using a published language based on ACR/EULAR definitions. The Autoimmune Disease Context and Joint Disease Context consume these results to drive their clinical logic. The Outcomes Tracking Context records assessment data for longitudinal analysis. A shared kernel with the Autoimmune Disease Context defines common joint examination terminology.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Prevoo, M.L., et al. "Modified Disease Activity Scores That Include Twenty-Eight-Joint Counts." Arthritis & Rheumatism, 1995.
- Aletaha, D., et al. "Acute Phase Reactants Add Little to Composite Disease Activity Indices." Arthritis Research & Therapy, 2005.
- Anderson, J., et al. "Rheumatoid Arthritis Disease Activity Measures: ACR Recommendations." Arthritis Care & Research, 2012.
