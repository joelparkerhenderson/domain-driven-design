# Entities in Gastroenterology

## Overview

An entity is a domain object defined by its unique identity rather than its attributes.
Entities persist over time, and their attributes may change while their identity remains
constant. In gastroenterology, entities represent clinical concepts that have a
continuous lifecycle and must be uniquely distinguishable from one another.

## Entity Characteristics

Entities in the gastroenterology domain share several characteristics:

1. Each entity has a unique identifier that remains stable throughout its lifecycle.
2. Two entities with identical attributes but different identifiers are distinct objects.
3. Entities encapsulate behavior that modifies their state in domain-meaningful ways.
4. Entity state changes must respect the invariants of the enclosing aggregate.

## Clinical Assessment Context Entities

### PatientEncounter

A PatientEncounter represents a specific clinical visit. Its identity is an encounter
ID assigned at creation. Over time, the encounter accumulates chief complaint details,
examination findings, assessment entries, and plan items. Two encounters for the same
patient on the same day are distinct entities because each represents a separate
clinical interaction. The entity enforces rules about what constitutes a complete
encounter and manages transitions through encounter states (scheduled, in-progress,
documented, signed).

### ClinicalAssessment

A ClinicalAssessment captures the structured evaluation performed during an encounter.
It has its own identity because the same encounter may generate multiple assessments
(initial assessment, reassessment after test results). The entity tracks assessment
type, findings, severity indicators, and the resulting clinical impression.

## Endoscopic Procedures Context Entities

### EndoscopicProcedure

An EndoscopicProcedure represents a specific procedure performed on a specific date.
Its identity is a procedure ID. The entity tracks procedure type (EGD, colonoscopy,
ERCP, EUS, capsule endoscopy), indication, findings, interventions, and quality
metrics. The entity manages state transitions from scheduled through preparation,
in-progress, recovery, and documented states.

### SpecimenRecord

A SpecimenRecord represents a specific tissue sample or fluid collection obtained
during a procedure. Each specimen has a unique accession number. The entity tracks
collection site, fixative, processing status, and links to pathology results. Multiple
specimens from the same procedure are distinct entities because each may yield
different pathology findings.

## Inflammatory Disease Context Entities

### IBDDiagnosis

An IBDDiagnosis represents a patient's inflammatory bowel disease diagnosis. It has
identity because the diagnosis evolves over time: classification may be refined,
disease extent may change, and phenotype may progress. The entity maintains the
current Montreal Classification and disease activity history.

### TherapyRegimen

A TherapyRegimen represents a specific treatment plan for IBD. Its identity persists
through dose adjustments, drug holidays, and therapy switches. The entity tracks the
therapeutic agent, dosing schedule, monitoring requirements, and efficacy assessments
over time.

## Hepatology Context Entities

### LiverDiseaseDiagnosis

A LiverDiseaseDiagnosis represents the ongoing characterization of a patient's liver
disease. The entity tracks etiology (viral, alcoholic, metabolic, autoimmune), fibrosis
stage, and complications. As the disease progresses, the entity's attributes change
while its identity remains stable, enabling longitudinal tracking.

### CirrhosisStaging

A CirrhosisStaging entity maintains the current severity classification of a cirrhotic
patient. It recalculates Child-Pugh and MELD scores as component lab values update.
The entity tracks decompensation events and transitions between compensated and
decompensated states.

## Motility Disorders Context Entities

### ManometryStudy

A ManometryStudy represents a specific motility study session. The entity captures
high-resolution manometry data, calculated metrics (integrated relaxation pressure,
distal contractile integral), and the resulting Chicago Classification diagnosis.

### PhMonitoringSession

A PhMonitoringSession represents an ambulatory pH or impedance-pH monitoring study.
The entity tracks total acid exposure time, DeMeester score, symptom correlation
indices (symptom index, symptom association probability), and the temporal correlation
between symptoms and reflux events.

## Nutrition Management Context Entities

### NutritionalAssessment

A NutritionalAssessment represents a specific evaluation of a patient's nutritional
status at a point in time. It captures anthropometric data, biochemical markers,
dietary intake analysis, and risk classification. Serial assessments for the same
patient are distinct entities that together form a nutritional trajectory.

### NutritionPlan

A NutritionPlan represents a specific nutritional intervention prescription. Its
identity persists through modifications to formulation, caloric targets, and
delivery method. The entity tracks plan creation, modifications, adherence data,
and outcomes.

## Identity Generation

Entity identifiers in the gastroenterology domain use universally unique identifiers
(UUIDs) generated at entity creation time. This avoids dependency on database sequences
and allows entities to be created before persistence. Clinical identifiers such as
medical record numbers or accession numbers serve as secondary identifiers for
integration purposes but do not replace the domain-level UUID identity.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 5: Entities.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 5: Entities.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8: Domain Model Patterns.
