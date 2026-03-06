# Hepatology Context

## Overview

The Hepatology Context manages the diagnosis, staging, treatment, and monitoring of
liver diseases within the gastroenterology domain. This context covers viral hepatitis
(A, B, C, D, E), autoimmune hepatitis, alcoholic and non-alcoholic liver disease,
cirrhosis staging and complication management, and liver transplant evaluation. Liver
disease management requires precise staging, continuous monitoring, and structured
evaluation protocols that this bounded context encapsulates.

## Scope and Responsibilities

This context is responsible for:

- Diagnosing and classifying liver disease by etiology and severity.
- Tracking viral hepatitis treatment and viral load monitoring.
- Staging cirrhosis using Child-Pugh and MELD scoring systems.
- Managing cirrhosis complications: ascites, variceal bleeding, hepatic encephalopathy,
  hepatorenal syndrome, and spontaneous bacterial peritonitis.
- Screening for hepatocellular carcinoma in at-risk populations.
- Conducting structured transplant candidacy evaluations.
- Coordinating transplant listing with organ allocation networks.
- Tracking disease progression through serial laboratory and imaging data.

## Core Aggregates

### LiverDiseaseDiagnosis

The primary aggregate representing a patient's liver disease characterization. Contains
the disease etiology (viral, alcoholic, metabolic/NAFLD/NASH, autoimmune, cholestatic,
drug-induced), fibrosis staging (F0-F4 or equivalent), current Child-Pugh and MELD
scores, active complications, and treatment history.

Key invariants: fibrosis staging must use a validated assessment method (biopsy,
elastography, or validated serum markers). Child-Pugh and MELD scores must be
recalculated when component laboratory values are updated. Complication management
follows established clinical algorithms.

### TransplantEvaluation

Manages the structured evaluation process for liver transplant candidacy. Contains
medical evaluation results, surgical assessment, cardiac clearance, psychosocial
evaluation, substance use assessment, financial and insurance evaluation, and the
final candidacy determination. Each evaluation component has completion status and
findings.

Key invariant: a listing recommendation cannot be issued until all required evaluation
components are completed. Contraindications identified in any component must be
explicitly addressed in the final determination.

## Key Entities

- CirrhosisStaging: maintains current severity classification with Child-Pugh class
  and MELD score, tracking transitions between compensated and decompensated states.
- HepatitisManagement: tracks viral load, treatment regimen, and sustained virologic
  response for viral hepatitis patients.
- ComplicationEpisode: documents individual complication events with severity, treatment,
  and resolution.

## Key Value Objects

- MELDScore: calculated from bilirubin, creatinine, INR, and sodium with the standard
  formula, producing a score predicting 90-day mortality.
- ChildPughScore: composite score from bilirubin, albumin, INR, ascites grade, and
  encephalopathy grade, classifying cirrhosis as A (5-6), B (7-9), or C (10-15).
- FibrosisStage: fibrosis severity from F0 (none) through F4 (cirrhosis) with the
  assessment method documented.
- ViralLoad: quantitative viral measurement with detection limit and log value.
- LiverFunctionPanel: composite value object containing AST, ALT, alkaline phosphatase,
  bilirubin, albumin, and INR.
- ElastographyResult: liver stiffness measurement in kPa with fibrosis stage correlation.

## Domain Events

- DiseaseProgressionDetected: raised when staging indicates worsening liver disease,
  triggering increased surveillance and potential transplant evaluation.
- DecompensationEventOccurred: raised when a new cirrhosis complication appears,
  triggering urgent clinical response.
- TransplantCandidacyDetermined: raised when the evaluation concludes with a candidacy
  decision.
- ViralClearanceAchieved: raised when sustained virologic response is confirmed in
  hepatitis treatment.
- HCCScreeningDue: raised when hepatocellular carcinoma screening interval is reached.

## Clinical Workflows

### Cirrhosis Staging Workflow

When cirrhosis is diagnosed, the CirrhosisStaging entity is created with initial
Child-Pugh and MELD scores. As laboratory values update, scores are recalculated
by the MELDScoreCalculationService. Score changes that cross clinical thresholds
trigger DiseaseProgressionDetected events.

### Transplant Evaluation Workflow

When a patient meets criteria for transplant consideration (MELD score, complications,
quality of life), the TransplantEvaluation aggregate is initiated. Each evaluation
component is completed independently. The TransplantCandidacyAssessmentService
aggregates results to determine candidacy. The TransplantCandidacyDetermined event
communicates the outcome.

### Variceal Surveillance Workflow

Patients with cirrhosis undergo screening endoscopy for esophageal varices. This
workflow coordinates with the Endoscopic Procedures Context, receiving EGD findings
and determining surveillance intervals based on variceal grade and liver disease
severity.

### Hepatitis Treatment Workflow

Viral hepatitis patients receive treatment with serial viral load monitoring. The
HepatitisManagement entity tracks treatment milestones and viral kinetics. Achievement
of sustained virologic response raises the ViralClearanceAchieved event.

## Integration Points

- Upstream: receives ERCP and EUS findings from Endoscopic Procedures Context.
- Upstream: receives laboratory results from Clinical Assessment Context.
- Downstream: communicates dietary requirements to Nutrition Management Context.
- External: coordinates with transplant networks for organ allocation.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Kamath, P.S. & Kim, W.R. (2007). "The MELD score." Hepatology.
- Pugh, R.N. et al. (1973). "Assessment of severity of liver disease." British Journal of Surgery.
- AASLD Practice Guidelines for various liver diseases (American Association for the Study of Liver Diseases).
