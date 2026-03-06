# Aggregates in Gastroenterology

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for the
purpose of data consistency. Each aggregate has a root entity that controls access to
all objects within the cluster and enforces invariants that must be satisfied at the
end of every transaction. In gastroenterology, aggregates model clinical concepts
that must maintain internal consistency.

## Aggregate Design Principles

Aggregates in the gastroenterology domain follow key design principles from DDD:

1. Each aggregate has exactly one root entity that serves as the entry point.
2. External references to the aggregate target only the root entity.
3. Invariants within the aggregate boundary are enforced on every state change.
4. Aggregates are designed to be as small as possible while maintaining consistency.
5. Cross-aggregate references use identity (IDs) rather than direct object references.

## Clinical Assessment Context Aggregates

### PatientEncounter Aggregate

Root entity: PatientEncounter. Contains the chief complaint, review of systems
responses, physical examination findings, and assessment plan. The invariant is that
a completed encounter must have at minimum a chief complaint, at least one assessment
finding, and a documented plan. Lab orders and imaging referrals reference the
encounter by ID but are separate aggregates to allow independent lifecycle management.

### LabOrder Aggregate

Root entity: LabOrder. Contains ordered tests, specimen collection status, and result
entries. The invariant is that a lab order cannot be marked complete until all ordered
tests have results. Each result is a value object within the aggregate.

## Endoscopic Procedures Context Aggregates

### EndoscopicProcedure Aggregate

Root entity: EndoscopicProcedure. Contains procedure type, indication, sedation record,
procedural findings, therapeutic interventions, and quality metrics. The invariant is
that a completed procedure must have documented informed consent, sedation records,
at least one finding entry, and calculated quality metrics (e.g., withdrawal time for
colonoscopy). Specimen records are separate aggregates linked by procedure ID.

### SpecimenRecord Aggregate

Root entity: SpecimenRecord. Contains specimen type, collection site, fixative type,
and pathology tracking status. The invariant is that every collected specimen must have
a documented collection site and be traceable through the pathology workflow.

## Inflammatory Disease Context Aggregates

### IBDDiagnosis Aggregate

Root entity: IBDDiagnosis. Contains disease type (Crohn's or UC), location classification
(Montreal Classification), disease behavior, and activity score history. The invariant
is that disease classification must follow Montreal Classification rules and activity
scores must use validated instruments (Harvey-Bradshaw Index or Mayo Score).

### TherapyRegimen Aggregate

Root entity: TherapyRegimen. Contains current and historical biologic therapies,
conventional immunomodulators, dose adjustments, and monitoring schedules. The invariant
is that active therapy changes must have documented rationale and that monitoring
intervals comply with prescribing requirements.

### FlareEpisode Aggregate

Root entity: FlareEpisode. Contains flare onset date, severity classification, symptom
documentation, treatment response, and resolution status. The invariant is that flare
severity must be scored using standardized criteria.

## Hepatology Context Aggregates

### LiverDiseaseDiagnosis Aggregate

Root entity: LiverDiseaseDiagnosis. Contains etiology, fibrosis staging, cirrhosis
classification, and complication tracking. The invariant is that staging scores must
be recalculated when component lab values change.

### TransplantEvaluation Aggregate

Root entity: TransplantEvaluation. Contains candidacy criteria, contraindication
screening, psychosocial evaluation, and listing recommendation. The invariant is that
listing recommendations require completed evaluations in all required domains.

## Motility Disorders Context Aggregates

### ManometryStudy Aggregate

Root entity: ManometryStudy. Contains pressure measurements, Chicago Classification
parameters, and diagnostic interpretation. The invariant is that classification must
follow the current Chicago Classification version.

### MotilityDiagnosis Aggregate

Root entity: MotilityDiagnosis. Contains Rome IV classification, diagnostic criteria
fulfillment, and treatment plan. The invariant is that diagnosis must satisfy the
specific Rome IV symptom duration and frequency thresholds.

## Nutrition Management Context Aggregates

### NutritionalAssessment Aggregate

Root entity: NutritionalAssessment. Contains anthropometric measurements, biochemical
markers, dietary intake analysis, and malnutrition risk score. The invariant is that
risk classification must follow validated screening tools.

### NutritionPlan Aggregate

Root entity: NutritionPlan. Contains enteral or parenteral formulation, caloric targets,
macro and micronutrient goals, and monitoring schedule. The invariant is that caloric
and protein targets must align with clinical condition and body composition.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 6: The Lifecycle of a Domain Object.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 10: Aggregates.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8: Aggregates.
