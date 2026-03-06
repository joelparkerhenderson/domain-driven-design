# Aggregates in Oncology

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for data changes. Each aggregate has a root entity that controls access to the aggregate's internals and enforces consistency rules (invariants). In oncology, aggregates model the key clinical concepts that must maintain internal consistency, such as a cancer diagnosis with its associated staging and molecular profile, or a chemotherapy order with its calculated doses and administration schedule.

## Aggregate Design Principles

Aggregates in oncology follow the standard DDD principles with domain-specific considerations. The aggregate boundary defines the scope of a single transaction. All changes within an aggregate are committed atomically, ensuring that the aggregate is always in a consistent state. References between aggregates use identity (IDs) rather than direct object references, enabling each aggregate to be loaded and persisted independently.

In oncology, aggregate boundaries must be drawn carefully to balance consistency requirements against performance and concurrency needs. A chemotherapy order and its dose calculations must be consistent within a single transaction, so they belong in the same aggregate. However, a chemotherapy order and the patient's overall treatment plan can be eventually consistent, so they belong in separate aggregates connected by domain events.

## Key Aggregates by Bounded Context

### Diagnosis and Staging Context

**Cancer Diagnosis Aggregate**: The root entity is the Cancer Diagnosis, which encapsulates the confirmed histological type, tumor grade, TNM classification, overall stage group, and molecular profile. The aggregate ensures that the TNM classification and overall stage are always consistent with each other according to AJCC rules. When a molecular profiling result is added, the aggregate validates that it applies to the correct tumor type.

**Pathology Case Aggregate**: The root entity is the Pathology Case, which manages the collection of specimens, tissue blocks, and pathology report sections for a single diagnostic evaluation. The aggregate ensures that the final pathology report accurately reflects all specimen findings and that amendments properly reference prior versions.

### Treatment Planning Context

**Treatment Plan Aggregate**: The root entity is the Treatment Plan, which captures the treatment intent (curative or palliative), selected modalities and their sequencing, and the tumor board decision that approved the plan. The aggregate ensures that the modality sequence is clinically valid (for example, neoadjuvant chemotherapy must precede surgery in the plan timeline) and that all selected modalities have been reviewed by the appropriate specialists.

### Chemotherapy Management Context

**Chemotherapy Order Aggregate**: The root entity is the Chemotherapy Order, which contains the selected regimen, patient-specific dose calculations for each drug, the administration schedule for each cycle, and pre-medication orders. The aggregate enforces dose limit invariants, ensures that dose modifications are properly documented with clinical justification, and validates that the total number of planned cycles does not exceed protocol limits.

**Toxicity Assessment Aggregate**: The root entity is the Toxicity Assessment, which records adverse events observed during or after chemotherapy administration, graded using the CTCAE scale. The aggregate ensures that each adverse event has a valid CTCAE grade and that the assessment is associated with the correct treatment cycle.

### Radiation Therapy Context

**Radiation Treatment Plan Aggregate**: The root entity is the Radiation Treatment Plan, which defines target volumes (GTV, CTV, PTV), organs at risk with dose constraints, the prescribed dose and fractionation schedule, and the delivery technique. The aggregate ensures that dose constraints for organs at risk are not violated and that the fractionation schedule delivers the prescribed total dose.

### Surgical Oncology Context

**Surgical Case Aggregate**: The root entity is the Surgical Case, which captures the planned procedure, actual procedure performed, intraoperative findings, margin assessment results, and lymph node evaluation. The aggregate ensures that margin assessment results are recorded for all resected specimens and that the surgical pathology findings are consistent with the operative report.

### Survivorship Care Context

**Survivorship Care Plan Aggregate**: The root entity is the Survivorship Care Plan, which contains the treatment summary (all treatments received), the recommended follow-up schedule, late effects to monitor, and wellness recommendations. The aggregate ensures that the follow-up schedule is consistent with the treatment history and current guidelines.

## Aggregate Invariants

Each aggregate enforces specific invariants that represent business rules which must always be true. Examples in oncology include: a TNM classification must contain valid values for T, N, and M categories as defined by the AJCC for the specific cancer type; a chemotherapy dose cannot exceed the protocol-defined maximum dose without explicit clinical override documentation; a radiation treatment plan must have at least one target volume defined; a surgical case cannot be marked complete without margin assessment results.

## Aggregate Size

Aggregates in oncology should be kept as small as possible while still maintaining the necessary consistency boundaries. A common mistake is to create a large "Patient" aggregate that contains everything about the patient's cancer care. Instead, the patient's care is distributed across multiple aggregates in different bounded contexts, connected by patient identity. This enables independent evolution of each aggregate and prevents contention when multiple clinical teams are working with different aspects of the same patient's care simultaneously.

## Inter-Aggregate Communication

Aggregates communicate through domain events. When a Cancer Diagnosis aggregate confirms a new staging result, it publishes a DiagnosisConfirmed event. The Treatment Plan aggregate in another context may subscribe to this event and initiate a treatment planning workflow. This eventual consistency model reflects the clinical reality that treatment planning does not happen instantaneously upon diagnosis confirmation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6: The Life Cycle of a Domain Object.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10: Aggregates.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9: Aggregates.
