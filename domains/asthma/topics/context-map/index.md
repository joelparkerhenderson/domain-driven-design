# Context Map - Asthma Domain

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a domain. In the asthma management domain, the context map documents how the six bounded contexts -- Clinical Assessment, Treatment Management, Trigger Monitoring, Action Planning, Medication Management, and Outcomes Tracking -- exchange information, share concepts, and maintain their boundaries.

The context map makes integration patterns explicit, revealing upstream/downstream relationships, translation requirements, and potential coupling risks. It serves as the primary strategic design artifact for communicating system architecture to both technical teams and domain experts.

## Context Relationships

### Clinical Assessment --> Treatment Management (Customer-Supplier)

The Clinical Assessment Context is upstream; the Treatment Management Context is downstream. Assessment results (severity classification, control level, spirometry data) flow downstream to inform treatment decisions. The Treatment Management team is the "customer" that negotiates the format and content of assessment data with the Clinical Assessment "supplier."

Translation: Treatment Management translates GINA severity classifications into stepwise therapy step recommendations. An FEV1 value object in Clinical Assessment maps to a treatment eligibility criterion in Treatment Management.

### Clinical Assessment --> Action Planning (Conformist)

The Action Planning Context conforms to the Clinical Assessment model for PEF personal best values and control level classifications. Action plan zone thresholds are derived directly from clinical assessment data, so the Action Planning Context adopts the Clinical Assessment model without translation.

### Treatment Management --> Medication Management (Customer-Supplier)

The Treatment Management Context is upstream, publishing treatment plan changes that drive prescription generation in the Medication Management Context. When a treatment plan step changes, the Medication Management Context creates, modifies, or discontinues prescriptions accordingly.

Translation: A stepwise therapy adjustment in Treatment Management maps to specific medication prescription changes (drug, dose, frequency, device) in Medication Management.

### Treatment Management --> Action Planning (Published Language)

The Treatment Management Context publishes treatment instructions in a standardized published language that the Action Planning Context consumes. This published language defines medication names, dosing ranges, and escalation instructions in a format suitable for patient-facing action plans.

### Trigger Monitoring --> Action Planning (Customer-Supplier)

The Trigger Monitoring Context is upstream, publishing environmental alerts (high AQI, elevated pollen counts, workplace exposure events) that the Action Planning Context uses to activate precautionary measures or zone changes.

### Trigger Monitoring --> Outcomes Tracking (Published Language)

The Trigger Monitoring Context publishes exposure data in a standardized format that the Outcomes Tracking Context uses to correlate environmental factors with exacerbation events and outcome trends.

### Medication Management --> Outcomes Tracking (Customer-Supplier)

The Medication Management Context is upstream, supplying adherence data, rescue inhaler usage counts, and prescription fulfillment records to the Outcomes Tracking Context for longitudinal analysis.

### Outcomes Tracking --> Treatment Management (Customer-Supplier)

The Outcomes Tracking Context publishes outcome trend alerts (declining ACT scores, increasing exacerbation frequency, declining FEV1 trend) upstream to the Treatment Management Context, triggering treatment plan review.

### Clinical Assessment --> Outcomes Tracking (Shared Kernel)

These two contexts share a small kernel of lung function value objects (FEV1, FVC, PEF measurements) to ensure consistent representation of pulmonary function data across diagnostic assessment and longitudinal tracking.

## External System Integration

### Electronic Health Record (EHR) Systems - Anti-Corruption Layer

All bounded contexts interact with external EHR systems through anti-corruption layers. The EHR model (HL7 FHIR resources, CDA documents) is translated into domain-specific models at the boundary. The Clinical Assessment Context translates FHIR Observation resources into spirometry domain objects. The Medication Management Context translates FHIR MedicationRequest resources into prescription domain objects.

### Air Quality and Allergen Data Services - Anti-Corruption Layer

The Trigger Monitoring Context integrates with external air quality APIs (EPA AirNow, commercial providers) and pollen count services through an anti-corruption layer that normalizes varying data formats into the domain's environmental exposure model.

### Pharmacy Systems - Open Host Service

The Medication Management Context exposes an open host service for pharmacy system integration, providing a standardized API for prescription transmission, refill notifications, and dispensing confirmations.

## Upstream/Downstream Summary

| Upstream Context         | Downstream Context        | Pattern            |
|--------------------------|---------------------------|--------------------|
| Clinical Assessment      | Treatment Management      | Customer-Supplier  |
| Clinical Assessment      | Action Planning           | Conformist         |
| Clinical Assessment      | Outcomes Tracking         | Shared Kernel      |
| Treatment Management     | Medication Management     | Customer-Supplier  |
| Treatment Management     | Action Planning           | Published Language |
| Trigger Monitoring       | Action Planning           | Customer-Supplier  |
| Trigger Monitoring       | Outcomes Tracking         | Published Language |
| Medication Management    | Outcomes Tracking         | Customer-Supplier  |
| Outcomes Tracking        | Treatment Management      | Customer-Supplier  |

## Design Considerations

**Feedback Loops.** The context map reveals a feedback loop: Clinical Assessment informs Treatment Management, which drives Medication Management, which feeds Outcomes Tracking, which triggers Treatment Management review, which may prompt new Clinical Assessment. This cycle mirrors the clinical reality of iterative asthma management.

**Single Direction of Conformity.** The Action Planning Context is intentionally conformist to Clinical Assessment to avoid introducing translation errors in safety-critical zone threshold calculations.

**Shared Kernel Minimization.** The shared kernel between Clinical Assessment and Outcomes Tracking is kept deliberately small (lung function measurement value objects only) to minimize coupling while ensuring data consistency.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
