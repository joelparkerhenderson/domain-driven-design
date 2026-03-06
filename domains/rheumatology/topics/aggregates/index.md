# Aggregates in the Rheumatology Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for data consistency. Each aggregate has a root entity that controls access to its internal objects and enforces invariants. In the rheumatology domain, aggregates encapsulate the complex relationships between clinical assessments, disease management plans, therapy regimens, pain interventions, and outcome records. Proper aggregate design ensures that clinical data integrity is maintained even under concurrent access by multiple clinicians.

## Clinical Assessment Context Aggregates

### PatientAssessment Aggregate

The PatientAssessment is the primary aggregate root in the Clinical Assessment Context. It encapsulates a single clinical evaluation episode and contains JointExamination entities (documenting tender and swollen joint counts for each examined joint group), SerologyResult value objects (RF titer, anti-CCP level, ANA titer and pattern), DiseaseActivityScore value objects (DAS28, CDAI, SDAI calculations), and PhysicalExamination findings.

The aggregate enforces invariants such as: a DAS28 score cannot be calculated unless all required components are present; serology results must include method and reference range; and joint counts must specify the examination scheme used (28-joint or 66/68-joint count). The PatientAssessment aggregate is created when a clinician begins an evaluation and finalized when the assessment is completed, at which point an AssessmentCompleted domain event is published.

The consistency boundary ensures that all examination findings, lab results, and calculated scores for a single visit are committed together. Partial assessments are valid in-progress states but cannot be used for clinical decision-making until finalized.

## Autoimmune Disease Context Aggregates

### DiseaseManagementPlan Aggregate

The DiseaseManagementPlan aggregate root manages the ongoing treatment strategy for an autoimmune disease. It contains the AutoimmuneDisease entity (with classification criteria results), TreatmentGoal value objects (defining the treat-to-target objective), MedicationRegimen entities (current and historical DMARD therapy), and FlareEpisode entities (documenting disease flare events and responses).

Key invariants include: a management plan must have an active classification before treatment can begin; treatment escalation requires documented failure of current therapy; and flare episodes must trigger a management plan review. The aggregate boundary is drawn around a single disease for a single patient, meaning a patient with both RA and lupus would have two separate DiseaseManagementPlan aggregates.

## Joint Disease Context Aggregates

### JointDiseasePlan Aggregate

The JointDiseasePlan aggregate root manages non-autoimmune joint disease treatment. It contains JointDisease entities (osteoarthritis staging, gout diagnosis), SynovialFluidAnalysis value objects (crystal identification, cell count), InjectionRecord entities (documenting joint injections), and UrateLoweringTherapy entities (for gout management).

Invariants include: gout management plans must track serum urate levels against target; injection records must document the specific joint, approach, and medications used; and urate-lowering therapy dose adjustments require documented urate levels.

## Biologic Therapy Context Aggregates

### TherapyRegimen Aggregate

The TherapyRegimen aggregate root manages the lifecycle of biologic or targeted synthetic DMARD therapy. It contains BiologicPrescription entities (drug, dose, route, frequency), PreTreatmentScreening value objects (TB test, hepatitis panels, vaccination status), InfusionSession entities (for IV biologics), SafetyMonitoring entities (lab monitoring schedules and results), and BiosimilarSwitch entities (documenting transitions between reference and biosimilar products).

Critical invariants include: therapy cannot start without completed pre-treatment screening; infusion sessions must document pre-medication administration; biosimilar switches require documented clinical rationale; and safety monitoring must occur at prescribed intervals. This aggregate's consistency boundary is particularly important because it enforces safety rules that protect patients from adverse events.

## Pain Management Context Aggregates

### PainManagementPlan Aggregate

The PainManagementPlan aggregate root coordinates multimodal pain interventions. It contains PainAssessment value objects (VAS scores, pain location, character), PharmacologicIntervention entities (NSAIDs, corticosteroids, analgesics), TherapyReferral entities (physical therapy, occupational therapy), and FunctionalGoal value objects (measurable rehabilitation targets).

Invariants include: pain management plans must include at least one non-pharmacologic intervention; therapy referrals must specify functional goals; and opioid prescriptions must include risk assessment documentation.

## Outcomes Tracking Context Aggregates

### OutcomeReport Aggregate

The OutcomeReport aggregate root maintains the longitudinal outcome record. It contains OutcomeRecord entities (individual measurement events), DAS28Trajectory value objects (disease activity trend over time), HAQDIAssessment value objects (functional disability measurements), RadiographicScore value objects (Sharp/van der Heijde progression), and PatientReportedOutcome value objects (PROMIS, EQ-5D scores).

Invariants include: outcome records must reference validated measurement instruments; radiographic scores must include comparison with baseline; and treat-to-target assessments must document whether the target was achieved and what action was taken.

## Aggregate Design Principles

Aggregates in the rheumatology domain follow several design principles. They are sized to match clinical transaction boundaries, not database schemas. They reference other aggregates by identity rather than direct object reference. They publish domain events when significant state changes occur. They enforce all business invariants within their consistency boundary.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapters 5-6.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5.
- Vernon, Vaughn. "Effective Aggregate Design." DDD Community, 2011.
