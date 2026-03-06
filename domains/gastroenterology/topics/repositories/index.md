# Repositories in Gastroenterology

## Overview

A repository is an abstraction that provides the illusion of an in-memory collection
of aggregate roots. Repositories encapsulate the logic of storing, retrieving, and
searching for aggregates, keeping the domain model free from persistence concerns.
In the gastroenterology domain, repositories provide access to clinical aggregates
without exposing how or where the data is stored.

## Repository Design Principles

Repositories in the gastroenterology domain adhere to DDD principles:

1. One repository per aggregate root. Only aggregate roots have repositories.
2. The repository interface is defined in the domain layer using domain language.
3. The repository implementation resides in the infrastructure layer.
4. Repositories return fully reconstituted aggregates, not partial data projections.
5. Query methods use domain concepts, not database query language.
6. Repositories enforce aggregate boundaries by loading complete aggregates.

## Clinical Assessment Context Repositories

### PatientEncounterRepository

Provides access to PatientEncounter aggregates. Core operations include retrieval by
encounter ID, retrieval of all encounters for a patient within a date range, and
retrieval of encounters with unresolved alarm features. The repository reconstitutes
the full encounter aggregate including chief complaint, examination findings,
assessment entries, and plan items.

Query methods reflect clinical needs: FindEncountersWithAlarmFeatures returns encounters
that require urgent follow-up. FindRecentEncountersByPatient returns encounters within
a specified lookback period for clinical context review.

### LabOrderRepository

Provides access to LabOrder aggregates. Supports retrieval by order ID, retrieval of
pending orders for a patient, and retrieval of orders with results matching specific
criteria (e.g., abnormal liver function tests). The repository loads the complete
order with all test entries and results.

## Endoscopic Procedures Context Repositories

### EndoscopicProcedureRepository

Provides access to EndoscopicProcedure aggregates. Supports retrieval by procedure ID,
retrieval of procedures by patient and type, and retrieval of procedures requiring
pathology follow-up. The repository loads complete procedure records including
findings, interventions, quality metrics, and specimen references.

Query methods include FindProceduresAwaitingPathology for tracking outstanding
results and FindSurveillanceDueProcedures for identifying patients due for repeat
procedures based on guideline intervals.

### SpecimenRecordRepository

Provides access to SpecimenRecord aggregates. Supports retrieval by accession number,
retrieval of specimens by procedure, and retrieval of specimens with pending pathology
results. The repository enables chain-of-custody tracking from collection through
final pathology reporting.

## Inflammatory Disease Context Repositories

### IBDDiagnosisRepository

Provides access to IBDDiagnosis aggregates. Supports retrieval by diagnosis ID and
by patient. The repository loads the complete diagnosis including Montreal Classification,
activity score history, and current disease state. Query methods include
FindPatientsInActiveFlare for clinical dashboard support and
FindPatientsOnBiologicTherapy for monitoring population management.

### TherapyRegimenRepository

Provides access to TherapyRegimen aggregates. Supports retrieval by regimen ID, by
patient, and by therapy type. Query methods include FindRegimensRequiringMonitoring
for identifying patients due for lab monitoring of immunosuppressive therapy.

### FlareEpisodeRepository

Provides access to FlareEpisode aggregates. Supports retrieval by episode ID and by
patient. Query methods include FindActiveFlares for clinical prioritization and
FindFlaresBySeasonOrTrigger for epidemiological pattern analysis.

## Hepatology Context Repositories

### LiverDiseaseDiagnosisRepository

Provides access to LiverDiseaseDiagnosis aggregates. Supports retrieval by diagnosis
ID and by patient. Query methods include FindPatientsByFibrosisStage for population
management and FindPatientsWithProgressingDisease for proactive intervention.

### TransplantEvaluationRepository

Provides access to TransplantEvaluation aggregates. Supports retrieval by evaluation
ID and by patient. Query methods include FindPendingEvaluations for tracking
incomplete evaluations and FindCandidatesByMELDRange for transplant listing management.

## Motility Disorders Context Repositories

### ManometryStudyRepository

Provides access to ManometryStudy aggregates. Supports retrieval by study ID and by
patient. Query methods include FindStudiesByDiagnosis for research and quality
improvement and FindStudiesRequiringReview for worklist management.

### MotilityDiagnosisRepository

Provides access to MotilityDiagnosis aggregates. Supports retrieval by diagnosis ID
and by patient. Query methods include FindPatientsByRomeIVClassification for cohort
identification.

## Nutrition Management Context Repositories

### NutritionalAssessmentRepository

Provides access to NutritionalAssessment aggregates. Supports retrieval by assessment
ID and by patient. Query methods include FindPatientsAtNutritionalRisk for screening
program management and FindAssessmentsTrendingAdversely for early intervention.

### NutritionPlanRepository

Provides access to NutritionPlan aggregates. Supports retrieval by plan ID and by
patient. Query methods include FindActivePlansRequiringReview for plan management
and FindPlansByInterventionType for outcome analysis.

## CQRS Considerations

For read-heavy clinical workflows such as dashboards and population reports, the
repository pattern may be supplemented with dedicated read models following CQRS
principles. The repository serves the command side (writes), while optimized read
projections serve queries that do not require full aggregate reconstitution.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 6: Repositories.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 12: Repositories.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8: Repositories.
