# Prenatal Care Context

## Overview

The Prenatal Care Context is a bounded context within the gynecology domain that manages pregnancy-related care from confirmation through delivery. It models the longitudinal prenatal record, tracks maternal and fetal health across trimesters, manages risk stratification, and coordinates labor and delivery planning.

## Scope and Responsibility

This context owns the prenatal record model. Its responsibilities include:

- Creating a prenatal record upon pregnancy confirmation and establishing the estimated due date.
- Scheduling and documenting routine prenatal visits with clinical measurements and observations.
- Ordering and interpreting ultrasound assessments for fetal anatomy, growth, and well-being.
- Stratifying pregnancy risk based on maternal factors, obstetric history, and current findings.
- Escalating care when high-risk conditions are identified.
- Developing and maintaining the labor plan in collaboration with the patient.
- Coordinating with Screening Programs for prenatal screening tests.
- Coordinating with Surgical Services when cesarean delivery or other surgical interventions are planned.

## Ubiquitous Language

Key terms within this context:

- Prenatal Record: The comprehensive longitudinal record of a pregnancy from confirmation to delivery.
- Prenatal Visit: A scheduled clinical encounter during pregnancy for monitoring maternal and fetal health.
- Gestational Age: The duration of pregnancy measured in weeks and days from the last menstrual period or ultrasound dating.
- Ultrasound Assessment: A sonographic evaluation of fetal anatomy, growth, fluid volume, and placental status.
- High-Risk Pregnancy: A pregnancy with identified risk factors requiring intensified monitoring and specialist involvement.
- Risk Stratification: The process of classifying pregnancy risk level based on clinical and demographic factors.
- Labor Plan: A documented strategy for the anticipated mode, timing, and management of labor and delivery.
- Trimester: One of three defined periods of pregnancy (first: weeks 1-12, second: weeks 13-27, third: weeks 28-delivery).

## Aggregate: Prenatal Record

The Prenatal Record is the primary aggregate root. It contains:

- PrenatalVisit entities (collection) recording clinical data from each scheduled visit.
- UltrasoundAssessment value objects capturing imaging findings.
- RiskStratification value object reflecting the current risk classification with contributing factors.
- LaborPlan value object documenting the delivery strategy.
- GestationalAge value object calculated from dating information.

Key invariants:

- Gestational age must be internally consistent across all visit records and ultrasound assessments.
- Risk stratification must be updated whenever new risk factors are identified or resolved.
- The labor plan must be consistent with the current risk level (a high-risk pregnancy may require a different delivery approach).
- Ultrasound assessments must include gestational age at the time of imaging.
- A prenatal record cannot be closed without documenting the pregnancy outcome.

## Domain Events Published

- **PregnancyConfirmed**: Published when a prenatal record is created. Consumed by Screening Programs (to schedule prenatal screenings) and Patient Education (to begin prenatal education).
- **HighRiskIdentified**: Published when risk stratification escalates to high-risk. Consumed by Clinical Assessment (for specialist consultation) and Patient Education (for high-risk education).
- **LaborPlanFinalized**: Published when the labor plan is agreed upon. Consumed by Surgical Services (if cesarean is planned).
- **GestationalMilestoneReached**: Published at key gestational milestones (end of first trimester, viability, full term). Consumed by Screening Programs and Patient Education.

## Domain Events Consumed

- **DiagnosticImpressionFormed** (from Clinical Assessment): Incorporates diagnostic findings relevant to pregnancy into the prenatal record.
- **AbnormalResultDetected** (from Screening Programs): Triggers risk re-stratification and clinical follow-up.
- **EducationSessionCompleted** (from Patient Education): Records that prenatal education has been provided.

## Domain Services

- **RiskStratificationService**: Evaluates maternal risk factors to classify pregnancy risk level using validated scoring systems.
- **GestationalAgeDateService**: Calculates gestational age, reconciles dating discrepancies, and determines estimated due date.

## Value Objects

- GestationalAge, FetalMeasurement, AmnioticFluidIndex, BishopScore, RiskCategory.

## Integration Points

- Partnership with Clinical Assessment Context for pregnancy confirmation and specialist consultations.
- Upstream to Screening Programs Context via PregnancyConfirmed and GestationalMilestoneReached events.
- Upstream to Surgical Services Context via LaborPlanFinalized events.
- Shared kernel with Clinical Assessment for patient demographic and clinical history value objects.
- Anti-corruption layer for integration with external ultrasound and radiology systems.

## Trimester-Based Workflow

The prenatal care workflow is organized by trimester:

- First trimester: Pregnancy confirmation, dating ultrasound, initial risk assessment, early screening tests.
- Second trimester: Anatomy scan, glucose screening, continued risk monitoring, birth planning discussions.
- Third trimester: Growth monitoring, group B streptococcus screening, labor plan finalization, delivery preparation.

## Subdomain Classification

Prenatal Care is a core subdomain. It involves high clinical stakes, complex longitudinal monitoring, and individualized care planning that demands sophisticated domain modeling.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- ACOG. Guidelines for Perinatal Care, 8th edition.
- ACOG. Practice Bulletin No. 175: Ultrasound in Pregnancy, 2016.
