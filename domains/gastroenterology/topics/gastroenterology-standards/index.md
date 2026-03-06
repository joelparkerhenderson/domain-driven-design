# Gastroenterology Standards

## Overview

Gastroenterology clinical practice is governed by numerous standardized scoring systems,
diagnostic criteria, classification schemes, and quality benchmarks. These standards
directly inform the design of value objects, validation rules, and business logic
within the domain model. In DDD terms, clinical standards become part of the published
language and shape how value objects self-validate and how domain services apply
diagnostic algorithms.

## Scoring Systems

### MELD Score (Model for End-Stage Liver Disease)

The MELD score predicts 90-day mortality in patients with chronic liver disease and
is used to prioritize organ allocation for liver transplantation. The formula uses
serum bilirubin, serum creatinine, and INR. The MELD-Na variant incorporates serum
sodium. The score ranges from 6 to 40. In the domain model, the MELDScore value object
encapsulates this calculation, validates input ranges, and classifies the resulting
score into clinical severity categories.

Reference: Kamath, P.S. et al. (2001). "A model to predict survival in patients with
end-stage liver disease." Hepatology.

### Child-Pugh Score

The Child-Pugh score classifies cirrhosis severity using five clinical and laboratory
variables: bilirubin, albumin, INR, ascites, and encephalopathy. Each variable scores
1-3 points, yielding a total of 5-15. Class A (5-6) is compensated, Class B (7-9) is
significant functional compromise, and Class C (10-15) is decompensated. The
ChildPughScore value object validates all components and computes the correct class.

Reference: Pugh, R.N. et al. (1973). "Transection of the oesophagus for bleeding
oesophageal varices." British Journal of Surgery.

### Mayo Score (Ulcerative Colitis)

The Mayo Score assesses ulcerative colitis disease activity across four domains: stool
frequency (0-3), rectal bleeding (0-3), endoscopic findings (0-3), and physician global
assessment (0-3). Total score ranges from 0-12. Scores of 0-2 indicate remission,
3-5 mild activity, 6-10 moderate, and 11-12 severe. The partial Mayo Score excludes
the endoscopic component for clinic-based assessment.

Reference: Schroeder, K.W. et al. (1987). "Coated oral 5-aminosalicylic acid therapy
for mildly to moderately active ulcerative colitis." NEJM.

### Harvey-Bradshaw Index

The Harvey-Bradshaw Index (HBI) assesses Crohn's disease activity using five categories:
general well-being (0-4), abdominal pain (0-3), number of liquid stools per day,
abdominal mass (0-3), and complications (1 point each). Remission is below 5, mild
5-7, moderate 8-16, and severe above 16.

Reference: Harvey, R.F. & Bradshaw, J.M. (1980). "A simple index of Crohn's disease
activity." Lancet.

## Diagnostic Classification Systems

### Montreal Classification (IBD)

The Montreal Classification standardizes IBD phenotyping. For Crohn's disease: age at
diagnosis (A1-A3), location (L1-L4), and behavior (B1-B3 with optional perianal
modifier). For ulcerative colitis: extent (E1-E3). The MontrealClassification value
object enforces valid combinations and supports disease phenotype tracking over time.

Reference: Silverberg, M.S. et al. (2005). "Toward an integrated clinical, molecular
and serological classification of IBD." Canadian Journal of Gastroenterology.

### Chicago Classification (Esophageal Motility)

The Chicago Classification (currently v4.0) provides a hierarchical algorithm for
diagnosing esophageal motility disorders from high-resolution manometry data. It
defines specific thresholds for integrated relaxation pressure, distal contractile
integral, and distal latency to classify disorders such as achalasia (types I-III),
EGJ outflow obstruction, absent contractility, distal esophageal spasm, and
hypercontractile esophagus.

Reference: Yadlapati, R. et al. (2021). "Esophageal motility disorders on HRM:
Chicago Classification v4.0." Neurogastroenterology and Motility.

### Rome IV Criteria (Functional GI Disorders)

The Rome IV criteria provide diagnostic criteria for functional gastrointestinal
disorders based on symptom patterns. Key classifications include functional dyspepsia,
irritable bowel syndrome, functional constipation, and functional abdominal pain.
Each diagnosis requires specific symptom types, durations (typically 3-6 months),
and frequencies.

Reference: Drossman, D.A. (2016). "Functional GI Disorders: History, Pathophysiology,
Clinical Features and Rome IV." Gastroenterology.

## Quality Standards

### Endoscopy Quality Indicators

The ACG/ASGE quality indicators for colonoscopy include adenoma detection rate
(benchmark: greater than or equal to 25%), cecal intubation rate (greater than or equal to 95%),
adequate bowel preparation rate (greater than or equal to 85%), and appropriate use of
surveillance intervals. These benchmarks are encoded in the QualityMetricsCalculationService.

Reference: Rex, D.K. et al. (2015). "Quality indicators for colonoscopy." American
Journal of Gastroenterology.

### Lyon Consensus (GERD Diagnosis)

The Lyon Consensus establishes thresholds for conclusive GERD diagnosis from pH
monitoring: acid exposure time greater than 6% is conclusive, less than 4% is normal, and
4-6% is inconclusive. These thresholds are encoded in the PhMonitoringSession aggregate
validation.

Reference: Gyawali, C.P. et al. (2018). "Modern diagnosis of GERD: The Lyon
Consensus." Gut.

## Standards in Domain Model Design

Clinical standards inform domain model design in several ways:

1. Value objects validate against standard-defined ranges and thresholds.
2. Domain services implement standard-defined algorithms and classification logic.
3. Aggregate invariants enforce standard-required documentation elements.
4. Domain events trigger when standard-defined thresholds are crossed.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 9: Published Language.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 6: Value Objects.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 5: Published Language.
