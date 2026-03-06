# Entities in the Rheumatology Domain

## Overview

Entities are domain objects defined by their unique identity rather than their attributes. In the rheumatology domain, entities represent clinical objects that persist over time, change state, and must be individually trackable. A patient assessment, a biologic prescription, or a disease management plan each has a unique identity that distinguishes it from all other instances, even if two instances share identical attribute values at a given moment. Entity design captures the lifecycle and state transitions that characterize rheumatology clinical workflows.

## Clinical Assessment Context Entities

### JointExamination

A JointExamination entity represents a single structured examination of a patient's joints at a specific point in time. It has a unique identity (examination ID), a reference to the patient and clinician, the examination date, the examination scheme (28-joint or 66/68-joint), and the individual joint findings. Each joint in the examination scheme has a tender/not-tender and swollen/not-swollen status. The entity computes tender joint count (TJC) and swollen joint count (SJC) from individual findings.

The JointExamination transitions through states: initiated (clinician begins examining), in-progress (joints being recorded), and completed (all joints assessed). A completed examination is immutable and becomes a historical record. The identity persists so that subsequent examinations can be compared to prior ones.

### LabOrder

A LabOrder entity tracks the lifecycle of a laboratory test request from order to result. It captures the ordering clinician, the tests requested (RF, anti-CCP, ANA, CRP, ESR, complement levels), order date, specimen collection date, and result date. The entity transitions from ordered to collected to resulted. Results are attached as SerologyResult or InflammatoryMarker value objects.

## Autoimmune Disease Context Entities

### AutoimmuneDisease

The AutoimmuneDisease entity represents a diagnosed autoimmune condition for a specific patient. Its identity persists from initial classification through ongoing management. It records the disease type (RA, SLE, scleroderma, vasculitis, Sjogren's), the date of diagnosis, the classification criteria applied and score achieved, seropositive or seronegative status (for RA), and current disease activity state (remission, low, moderate, high).

This entity changes state as disease activity fluctuates. A disease in remission may transition to active during a flare. The entity maintains a history of disease activity states to support longitudinal analysis.

### MedicationRegimen

A MedicationRegimen entity tracks a specific medication prescription within a disease management plan. It records the drug name, class (csDMARD, bDMARD, tsDMARD), dose, route, frequency, start date, and discontinuation date with reason. The entity transitions through states: planned, active, on-hold (for adverse events or surgery), and discontinued.

## Joint Disease Context Entities

### JointDisease

The JointDisease entity represents a diagnosed joint condition such as osteoarthritis or gout. It records the disease type, affected joints, staging or classification, date of diagnosis, and current status. For osteoarthritis, it includes Kellgren-Lawrence grading. For gout, it includes tophaceous status and most recent serum urate level.

### JointInjection

A JointInjection entity documents a specific joint injection procedure. Its identity allows tracking of injection history for individual joints. It records the target joint, approach used, medications injected (corticosteroid type and dose, hyaluronic acid product), complications, and patient response. The entity supports tracking injection frequency to ensure appropriate intervals between procedures.

## Biologic Therapy Context Entities

### BiologicPrescription

The BiologicPrescription entity represents a specific biologic or targeted synthetic DMARD prescription. It records the drug (adalimumab, etanercept, infliximab, tocilizumab, tofacitinib, etc.), dose, route (subcutaneous or intravenous), frequency, indication, and prescribing clinician. The entity transitions through states: draft, pending-approval (for prior authorization), approved, active, suspended (for adverse events or intercurrent illness), and discontinued.

### InfusionSession

An InfusionSession entity documents a single infusion visit for intravenous biologic therapy. It records the scheduled drug and dose, pre-medications administered, infusion start time and rate adjustments, vital sign monitoring, infusion reaction events (if any), and completion status. The entity supports tracking cumulative infusion count for drugs with dose-escalation protocols.

## Pain Management Context Entities

### TherapyReferral

A TherapyReferral entity tracks a referral to a physical therapist or occupational therapist. It records the referring clinician, therapy type, diagnosis, functional limitations, specific goals, referral date, and appointment status. The entity transitions from created to accepted to in-progress to completed or cancelled. Progress reports from therapy providers are attached as the referral progresses.

## Outcomes Tracking Context Entities

### OutcomeRecord

An OutcomeRecord entity represents a single outcome measurement event. It records the patient, measurement date, instrument used (DAS28, HAQ-DI, Sharp/van der Heijde, PROMIS, EQ-5D), the measured values, the clinician who performed the assessment, and any clinical context (such as whether the measurement occurred during a flare or in remission).

The OutcomeRecord's identity allows it to participate in longitudinal trajectories. A series of OutcomeRecords for the same patient and instrument creates a trajectory that reveals disease progression or improvement over time.

## Entity Design Principles

Entities in the rheumatology domain follow several principles. Each entity has a stable, unique identifier that does not change over its lifetime. Entities encapsulate their state transitions and enforce validity rules at each transition. Entities are mutable but track their change history when clinically significant. References between entities across aggregate boundaries use identity references rather than direct object references.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5.
