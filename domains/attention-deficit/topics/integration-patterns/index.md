# Integration Patterns: Attention-Deficit Domain

Integration patterns define how bounded contexts in the ADHD management domain communicate, share data, and coordinate activities. These patterns establish the rules of engagement between clinical, educational, therapeutic, and pharmacological systems, balancing autonomy with interoperability.

## Purpose

The ADHD management domain inherently requires integration across diverse professional systems. Clinicians need treatment outcome data. Educators need clinical recommendations. Prescribers need behavioral monitoring data. Outcomes researchers need data from all contexts. Integration patterns make these data-sharing relationships explicit and manageable, preventing the creation of a distributed monolith where every context is tightly coupled to every other.

## Customer-Supplier Pattern

### Clinical Assessment to Treatment Management

The Clinical Assessment Context is the upstream supplier providing diagnostic data. The Treatment Management Context is the downstream customer consuming this data to create treatment plans. The supplier commits to providing a stable interface with diagnostic impression data including ADHD presentation type, severity level, comorbidity profile, and clinical recommendations. The customer's needs drive the supplier's interface design, ensuring that the diagnostic output format supports treatment planning workflows.

### Clinical Assessment to Medication Management

The Clinical Assessment Context supplies diagnostic confirmation and treatment recommendations to the Medication Management Context. The medication context, as customer, requires specific data elements: confirmed diagnosis, presentation type, severity rating, comorbidity screening results (particularly for conditions that influence medication selection such as anxiety, tics, and cardiac history), and any clinician recommendations regarding pharmacological approach.

### Treatment Management to Outcomes Tracking

The Treatment Management Context supplies treatment intervention records, including modality assignments, session frequencies, treatment goals, and episode timelines. The Outcomes Tracking Context, as customer, uses this data to correlate specific interventions with measured outcomes and to attribute treatment response to particular components of the multimodal plan.

## Partnership Pattern

### Treatment Management and Behavioral Monitoring

These two contexts operate in a partnership relationship where neither is purely upstream or downstream. The Treatment Management Context defines behavioral targets and monitoring parameters that the Behavioral Monitoring Context implements. Simultaneously, the Behavioral Monitoring Context provides ongoing behavioral data that drives treatment plan adjustments. Both contexts must evolve their interfaces collaboratively, as changes in one directly affect the other's operations.

This partnership requires regular coordination between the treatment planning team and the behavioral monitoring team. Changes to treatment goals necessitate corresponding changes to monitoring parameters, and significant behavioral findings necessitate treatment plan review.

## Conformist Pattern

### Educational Accommodation to External School Systems

The Educational Accommodation Context must conform to the data structures and workflows of external school information systems (SIS), special education management systems, and state reporting systems. These external systems are not within the ADHD management domain's control, and the accommodation context must adapt its outputs to match their expectations. This includes conforming to state-mandated IEP formats, district-specific 504 plan templates, and standardized reporting structures.

## Published Language Pattern

### Standardized Clinical Data Exchange

Multiple contexts share data through published language interfaces based on established clinical standards:

- **DSM-5 Diagnostic Codes**: The published language for diagnostic information, used by Clinical Assessment (producer), Treatment Management, Medication Management, and Educational Accommodation (consumers).
- **Rating Scale Score Formats**: Standardized scoring formats for Conners, SNAP-IV, Vanderbilt, and BRIEF scales, enabling consistent interpretation across Clinical Assessment, Behavioral Monitoring, and Outcomes Tracking.
- **Medication Coding Standards**: NDC (National Drug Code) and RxNorm coding for medication identification, ensuring unambiguous medication references across Medication Management and Outcomes Tracking.
- **Outcome Measurement Standards**: Standardized scoring and interpretation frameworks for PedsQL, WFIRS, and other outcome instruments used by the Outcomes Tracking Context.

## Open Host Service Pattern

### Medication Management Event Publication

The Medication Management Context exposes an open host service that publishes medication change events (MedicationTrialInitiated, DosageAdjusted, SideEffectReported, MedicationDiscontinued) in a standardized format. Any context that needs to react to medication changes can subscribe to this service without requiring bilateral agreements. The Behavioral Monitoring Context subscribes to correlate medication changes with behavioral patterns. The Outcomes Tracking Context subscribes to include medication data in treatment effectiveness analysis.

## Anti-Corruption Layer Pattern

### Clinical-to-Educational Translation

The most critical anti-corruption layer in the domain translates between clinical diagnostic terminology and educational eligibility and accommodation language. Clinical concepts such as "ADHD Combined Presentation, moderate severity with comorbid anxiety" must be translated into educational concepts such as "Other Health Impairment eligibility with accommodations for attention, organization, and anxiety management." This ACL protects both contexts from being corrupted by each other's conceptual model.

## Shared Kernel Pattern

### Patient Identity Kernel

A minimal shared kernel provides a common patient identity model used across all bounded contexts. This kernel includes only the elements necessary for cross-context correlation: a unique patient identifier, basic demographic attributes (name, date of birth), and active context references. The shared kernel is deliberately minimal to avoid creating coupling through shared domain logic.

## Separate Ways Pattern

### Educational Accommodation Independence

For day-to-day operations, the Educational Accommodation Context operates largely independently of clinical treatment contexts. Classroom accommodations are implemented by educators based on the established plan, without real-time coordination with clinicians or therapists. Alignment occurs periodically during scheduled IEP/504 review meetings, not through continuous integration.

## Integration Pattern Selection Criteria

Pattern selection depends on the relationship characteristics:

- **Frequency of interaction**: High-frequency data exchange favors published language and open host service. Infrequent interaction can use simpler point-to-point patterns.
- **Power dynamics**: When one context cannot influence the other's design, conformist or ACL patterns are appropriate.
- **Data sensitivity**: PHI and FERPA-protected data require secure integration channels with access controls.
- **Evolution rate**: Rapidly evolving contexts benefit from ACL protection to isolate change impact.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
