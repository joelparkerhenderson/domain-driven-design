# Integration Patterns - Asthma Domain

## Overview

Integration patterns define how bounded contexts interact with each other and with external systems. In Domain-Driven Design, integration patterns are strategic choices that determine the degree of coupling, the direction of influence, and the translation mechanisms between contexts. The asthma management domain employs multiple integration patterns selected based on the specific relationship between each pair of interacting contexts.

Choosing the right integration pattern is a strategic decision with long-term consequences. The wrong pattern can create unwanted coupling, contaminate domain models, or introduce fragility. The asthma domain's integration patterns are chosen to balance autonomy, consistency, and practical implementation constraints.

## Customer-Supplier Pattern

In a customer-supplier relationship, the upstream (supplier) context provides data or services that the downstream (customer) context depends on. The customer influences the supplier's API through negotiation, but the supplier retains control.

### Clinical Assessment (Supplier) --> Treatment Management (Customer)

The Treatment Management Context depends on assessment results to make treatment decisions. The Clinical Assessment Context publishes assessment data in a format negotiated with the Treatment Management team. Treatment Management can request changes to the assessment event schema (such as including additional biomarker data), but Clinical Assessment decides whether and how to implement those changes.

**Rationale:** Clinical Assessment cannot be subordinate to Treatment Management because diagnostic objectivity must be maintained. However, Treatment Management has legitimate needs that influence what assessment data is published.

### Treatment Management (Supplier) --> Medication Management (Customer)

Medication Management depends on treatment plan changes to generate prescriptions. Treatment Management publishes treatment decisions; Medication Management translates these into prescription actions.

**Rationale:** Treatment decisions drive prescriptions, not the reverse. Medication Management needs to receive treatment changes promptly and in a usable format.

### Trigger Monitoring (Supplier) --> Action Planning (Customer)

Action Planning consumes environmental alerts from Trigger Monitoring to activate precautionary measures. Trigger Monitoring publishes alerts based on patient-specific trigger profiles; Action Planning translates alerts into patient-facing notifications.

### Medication Management (Supplier) --> Outcomes Tracking (Customer)

Outcomes Tracking consumes adherence data and prescription fill records from Medication Management for longitudinal analysis.

### Outcomes Tracking (Supplier) --> Treatment Management (Customer)

Outcomes Tracking publishes trend alerts that Treatment Management consumes for proactive treatment review. This creates a feedback loop where outcomes inform treatment decisions.

## Conformist Pattern

In a conformist relationship, the downstream context adopts the upstream context's model without translation. The downstream context conforms to the upstream model because translation would introduce risk or unnecessary complexity.

### Clinical Assessment --> Action Planning (Conformist)

The Action Planning Context conforms to the Clinical Assessment model for peak expiratory flow personal best values and control level classifications. Zone thresholds in action plans are derived directly from clinical assessment data using the same value objects and calculation logic.

**Rationale:** Translating PEF values or control level classifications between models would introduce the risk of calculation errors in safety-critical zone threshold determination. Conforming to the authoritative clinical model eliminates this risk.

**Trade-off:** Action Planning is coupled to Clinical Assessment's model evolution. If Clinical Assessment changes how personal best is calculated, Action Planning must update accordingly. This coupling is accepted because zone threshold accuracy is a patient safety priority.

## Published Language Pattern

A published language is a standardized, well-documented data format shared between contexts. Unlike conformist relationships, published languages are explicitly designed for inter-context communication and documented as formal specifications.

### Treatment Management --> Action Planning (Published Language)

Treatment Management publishes treatment instructions in a standardized published language designed for patient-facing communication. This language defines:

- Medication names in both clinical and patient-friendly forms.
- Dosing instructions in patient-understandable format.
- Step change descriptions suitable for action plan integration.
- Escalation and de-escalation criteria in plain language.

**Rationale:** Treatment Management's internal model uses clinical terminology (ICS step 3, LABA adjunct therapy) that is not suitable for patient-facing action plans. The published language translates clinical concepts into patient-understandable instructions while maintaining clinical accuracy.

### Trigger Monitoring --> Outcomes Tracking (Published Language)

Trigger Monitoring publishes environmental exposure data in a standardized format that Outcomes Tracking uses for trigger-exacerbation correlation analysis. The published language defines:

- Standardized trigger type codes.
- Exposure intensity scales.
- Temporal resolution for exposure records.
- Geographic reference format.

## Shared Kernel Pattern

A shared kernel is a small, explicitly defined subset of the domain model shared between two bounded contexts. Both contexts depend on and co-own this shared code or model, and changes require agreement from both teams.

### Clinical Assessment <--> Outcomes Tracking (Shared Kernel)

These two contexts share a small kernel of lung function value objects:

- FEV1 value object (value, predicted, percent predicted).
- FVC value object (value, predicted, percent predicted).
- PEFReading value object (value, timestamp, context).
- PredictedValues value object (reference equations, patient demographics).

**Rationale:** Lung function measurements must be represented identically in diagnostic assessment and longitudinal tracking. Different representations would make trend analysis unreliable.

**Governance:** Changes to shared kernel value objects require agreement from both the Clinical Assessment and Outcomes Tracking teams. The shared kernel is deliberately kept as small as possible to minimize coupling.

## Anti-Corruption Layer Pattern

Anti-corruption layers (ACLs) protect bounded contexts from external system models. See the dedicated anti-corruption layer topic for detailed coverage. Key ACLs in the asthma domain include:

- **EHR Integration ACL** (Clinical Assessment): Translates FHIR resources to domain objects.
- **Air Quality Service ACL** (Trigger Monitoring): Normalizes external provider data formats.
- **Pharmacy System ACL** (Medication Management): Translates NCPDP SCRIPT messages.
- **Laboratory System ACL** (Clinical Assessment): Translates lab results.
- **Insurance/Payer ACL** (Medication Management): Handles prior authorization workflows.

## Open Host Service Pattern

An open host service exposes a bounded context's capabilities through a well-defined, versioned API that external systems can consume without requiring custom integration for each consumer.

### Medication Management Open Host Service

The Medication Management Context exposes an open host service for pharmacy system integration. This API supports:

- Prescription transmission (e-prescribing).
- Fill confirmation and notification.
- Formulary query.
- Prior authorization status inquiry.

**Rationale:** Multiple pharmacy systems need to interact with the Medication Management Context. Rather than building custom integrations for each pharmacy, the open host service provides a standardized interface.

### Outcomes Tracking Open Host Service

The Outcomes Tracking Context exposes an open host service for quality reporting systems. This API supports:

- Outcome metric queries (ACT scores, exacerbation rates, lung function trends).
- Population health reports.
- Quality measure calculations (HEDIS compliance).

## Separate Ways Pattern

The separate ways pattern applies when two contexts have no meaningful integration need. In the asthma domain, there are no pure separate ways relationships, but some context pairs have minimal interaction:

- Trigger Monitoring and Medication Management have no direct relationship. Environmental data does not directly affect prescription workflows, and medication adherence data does not directly affect trigger monitoring. Any indirect relationship flows through intermediate contexts (Trigger Monitoring to Action Planning, Medication Management to Outcomes Tracking).

## Integration Pattern Selection Guide

| Context Pair                                      | Pattern            | Rationale                                      |
|---------------------------------------------------|--------------------|-------------------------------------------------|
| Clinical Assessment --> Treatment Management      | Customer-Supplier  | Treatment needs assessment data; negotiated API |
| Clinical Assessment --> Action Planning           | Conformist         | Safety-critical data; no translation risk       |
| Clinical Assessment <--> Outcomes Tracking        | Shared Kernel      | Identical lung function representation needed   |
| Treatment Management --> Medication Management    | Customer-Supplier  | Treatment drives prescriptions                  |
| Treatment Management --> Action Planning          | Published Language | Clinical-to-patient language translation        |
| Trigger Monitoring --> Action Planning            | Customer-Supplier  | Alerts drive precautionary actions               |
| Trigger Monitoring --> Outcomes Tracking          | Published Language | Standardized exposure data for correlation      |
| Medication Management --> Outcomes Tracking       | Customer-Supplier  | Adherence data feeds outcome analysis           |
| Outcomes Tracking --> Treatment Management        | Customer-Supplier  | Trend alerts trigger treatment review           |

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context map patterns.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping and integration.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
