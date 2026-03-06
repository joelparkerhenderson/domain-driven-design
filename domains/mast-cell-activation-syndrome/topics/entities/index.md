# Entities for Mast Cell Activation Syndrome

## Overview

Entities are domain objects defined by their unique identity rather than their attributes. An entity persists over time and its attributes may change, but its identity remains constant. In the MCAS domain, entities represent clinical concepts that have distinct lifecycles, evolve over time, and must be individually trackable.

The distinction between entities and value objects is fundamental to correct domain modeling. An entity is chosen when the system must track and distinguish individual instances even if their attributes are identical. For example, two laboratory test results with the same tryptase value are still distinct entities because they represent different measurements taken at different times.

## Diagnostic Assessment Entities

### Diagnostic Workup

The Diagnostic Workup entity represents a complete diagnostic evaluation for a patient suspected of having MCAS. It has a unique identifier and progresses through states: initiated, in-progress, pending-interpretation, and concluded. Its attributes include the ordering clinician, the date initiated, the set of ordered tests, and the diagnostic conclusion. The workup's identity persists even as test results are added and the conclusion evolves.

### Mediator Panel

The Mediator Panel entity represents a set of laboratory tests ordered as a group to assess mast cell mediator levels. Each panel has a unique identity and tracks its own completion status. A single Diagnostic Workup may contain multiple Mediator Panels ordered at different times, such as a baseline panel and an acute panel collected during a flare.

### Bone Marrow Assessment

The Bone Marrow Assessment entity tracks a bone marrow biopsy procedure from scheduling through pathological interpretation. Its unique identity allows the system to associate the assessment with the ordering workup while maintaining its own lifecycle, as bone marrow assessments may take weeks to complete and involve multiple pathology reviews.

## Trigger Management Entities

### Trigger Profile

The Trigger Profile entity represents the evolving collection of identified triggers for an individual patient. Its identity is tied to the patient, and its content changes over time as new triggers are discovered and existing triggers are reclassified. The profile maintains a history of all trigger identifications and their evidence bases.

### Trigger Entry

Each Trigger Entry entity represents a single identified trigger within a patient's profile. It has a unique identity that allows the system to track its status over time. A trigger entry progresses through states: suspected, under-investigation, confirmed, and resolved. Its attributes include the trigger substance or condition, the evidence supporting identification, the confidence level, and the linked avoidance strategy.

### Avoidance Plan

The Avoidance Plan entity represents the comprehensive strategy for minimizing trigger exposure. It has its own lifecycle, being created after initial trigger identification and updated as new triggers are confirmed or existing avoidance strategies are refined. The plan entity coordinates environmental controls and dietary restrictions into a coherent whole.

## Medication Protocol Entities

### Medication Regimen

The Medication Regimen entity represents the complete set of medications prescribed for a patient's MCAS management. Its identity persists as medications are added, removed, or adjusted. The regimen tracks the history of all changes, enabling retrospective analysis of treatment evolution.

### Medication Entry

Each Medication Entry entity within a regimen represents a single prescribed medication. It has a unique identity and its own lifecycle from initiation through titration to steady state or discontinuation. Attributes include the medication name, dosage, frequency, route of administration, prescribing clinician, and current titration phase.

### Compounding Specification

The Compounding Specification entity represents the formulation instructions for a custom-compounded medication. Each specification has a unique identity because a patient may have multiple compounding specifications for different medications or different formulations of the same medication over time.

## Symptom Tracking Entities

### Symptom Log

The Symptom Log entity represents a bounded collection of symptom entries for a patient. Each log has a unique identity and covers a defined time period. Logs accumulate symptom entries over time and serve as the primary data source for pattern analysis.

### Flare Record

The Flare Record entity documents a single acute exacerbation episode. Each flare has a unique identity because patients experience multiple flares that must be individually tracked, compared, and analyzed. The flare record evolves as the episode progresses through onset, peak, and resolution phases.

## Specialist Coordination Entities

### Care Team

The Care Team entity represents the group of healthcare providers actively managing a patient's MCAS care. Its composition changes over time as specialists are added or removed. The care team entity maintains the current roster and the history of team membership changes.

### Referral

The Referral entity tracks a specialist consultation request through its lifecycle. Each referral has a unique identity and progresses through states: requested, accepted, scheduled, completed, and reported. Its attributes include the referring provider, target specialty, clinical reason, urgency level, and the consultation outcome.

### Shared Care Plan

The Shared Care Plan entity represents the coordinated treatment strategy agreed upon by the care team. It has a unique identity and evolves as the team refines treatment goals and interventions. The care plan maintains version history to track how the management approach has changed over time.

## Outcomes Measurement Entities

### Outcomes Assessment

The Outcomes Assessment entity represents a comprehensive evaluation of treatment effectiveness at a point in time. Each assessment has a unique identity and captures symptom burden scores, quality of life ratings, and functional status measurements. Assessments are designed to be comparable over time to reveal trends.

## Design Guidance

Entities in the MCAS domain should expose behavior through methods that enforce business rules rather than allowing arbitrary attribute modification. Identity generation should use domain-meaningful identifiers where possible and system-generated identifiers where clinical identifiers do not exist.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5 on entities and identity.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 4 on entities and value objects.
