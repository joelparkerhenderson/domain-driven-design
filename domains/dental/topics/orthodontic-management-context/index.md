# Orthodontic Management Context - Dental Domain

## Overview

The Orthodontic Management Context handles the diagnosis, treatment, and retention phases of orthodontic care. Orthodontic treatment operates on a fundamentally different timescale than other dental procedures, spanning months or years of incremental tooth movement. This context manages malocclusion assessment, appliance selection and management (braces and clear aligners), treatment progress tracking against predicted outcomes, and the retention phase that maintains achieved results. The extended timeline and progressive nature of orthodontic treatment require a distinct domain model optimized for longitudinal case tracking.

## Core Responsibilities

### Malocclusion Assessment

Malocclusion assessment evaluates the patient's occlusal relationship, dental alignment, and skeletal proportions to classify the orthodontic problem and determine treatment complexity. The assessment process includes:

- Angle classification of the molar relationship (Class I, II Division 1, II Division 2, III).
- Measurement of overjet (horizontal overlap) and overbite (vertical overlap).
- Assessment of dental crowding or spacing in millimeters.
- Cephalometric analysis using lateral cephalogram measurements to evaluate skeletal relationships.
- Study model or digital scan analysis for arch form, tooth size, and Bolton analysis.
- Facial profile assessment for aesthetic treatment planning.

The assessment produces a comprehensive orthodontic diagnosis with a complexity rating that informs treatment duration and appliance selection.

### Appliance Management

Appliance management tracks the orthodontic hardware used to achieve tooth movement. The two primary appliance categories are:

**Fixed Appliances (Braces)**: Brackets bonded to individual teeth connected by archwires. The context tracks bracket type (metal, ceramic, lingual), bracket prescription (torque, tip, rotation values), archwire sequence (progression from light flexible wires to heavier working wires), and auxiliary components (elastics, springs, power chains).

**Clear Aligner Systems**: Sequential removable trays that progressively move teeth. The context tracks the total number of aligners in the series, the current active aligner number, wear compliance, and staging of tooth movements across aligner sets. Aligner refinements (additional aligner sets to address remaining discrepancies) are tracked as case modifications.

### Progress Tracking

Progress tracking compares current tooth positions against the predicted treatment trajectory at each adjustment or monitoring visit. The context records:

- Clinical measurements of alignment, spacing, and occlusal relationship changes.
- Photographic records showing before, during, and after treatment comparisons.
- Assessment of whether individual teeth are tracking (moving as predicted) or lagging.
- Wire progression records for braces cases.
- Aligner fit verification for clear aligner cases.

When progress deviates significantly from predictions, the context supports treatment plan modifications including wire changes, bracket repositioning, aligner refinements, or timeline extensions.

### Retention Planning

Retention planning manages the post-active-treatment phase that prevents orthodontic relapse. The context tracks:

- Retainer type (fixed bonded wire, removable Hawley, removable clear retainer).
- Retainer delivery date and fit verification.
- Retention monitoring schedule (typically frequent initially, then decreasing over time).
- Compliance assessment at retention check appointments.
- Retainer replacement or repair events.

The retention phase may last indefinitely, with some patients requiring lifetime retainer wear to maintain results.

## Aggregate Roots

**Orthodontic Case**: The primary aggregate spanning the entire orthodontic treatment from assessment through retention. Contains assessment records, appliance specifications, adjustment visit history, progress measurements, and retention plan. Enforces that active treatment cannot begin without completed assessment and approved plan.

## Key Entities

- Orthodontic Appliance: The braces system or aligner set with type and specifications.
- Adjustment Visit: A single orthodontic adjustment appointment with wire changes and progress notes.
- Retention Device: The retainer with type, delivery date, and compliance tracking.

## Key Value Objects

- Angle Classification: Molar relationship classification (Class I, II, III).
- Cephalometric Measurement: Named measurement with value and normal range.
- Overjet Measurement: Horizontal overlap distance in millimeters.
- Overbite Measurement: Vertical overlap percentage or millimeters.
- Aligner Stage: Current aligner number relative to total count.

## Domain Events Produced

- OrthodonticCaseStarted: Triggers scheduling of recurring adjustment appointments.
- OrthodonticProgressRecorded: Updates the clinical chart with orthodontic status.
- OrthodonticTreatmentCompleted: Signals transition to retention and adjusts scheduling.
- RetainerDelivered: Records initiation of the retention phase.

## Domain Events Consumed

- TreatmentPlanApproved: Receives authorized orthodontic treatment for case initiation.
- InformedConsentObtained: Verifies consent before appliance placement.
- TreatmentPlanModified: Adjusts case parameters based on plan revisions.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Proffit, William R., Henry W. Fields, and David M. Sarver. Contemporary Orthodontics. Elsevier, 2018.
- Angle, Edward H. Classification of Malocclusion. Dental Cosmos, 1899.
