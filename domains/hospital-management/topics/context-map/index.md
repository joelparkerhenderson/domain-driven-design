# Context Map in Hospital Management

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a system. It documents how teams and their models relate, what integration patterns are used, and where translation boundaries exist. In hospital management, the context map clarifies how patient data flows from registration through clinical care to billing and beyond.

## Relevance to Hospital Management

Hospital systems involve multiple departments, external partners (insurance companies, laboratories, pharmacies), and regulatory bodies. A context map makes these relationships explicit, preventing hidden dependencies and enabling teams to understand the impact of changes across the system.

## Context Relationships

### Patient Context ↔ Clinical/EMR Context

- **Pattern**: Customer-Supplier
- **Direction**: Clinical/EMR (downstream customer) depends on Patient Context (upstream supplier) for patient identity and demographics
- **Integration**: Patient Context publishes PatientRegistered and PatientUpdated events; Clinical/EMR subscribes and maintains a local projection of patient identity
- **Translation**: Clinical context translates Patient demographics into its own PatientSummary value object

### Patient Context ↔ Billing/Administrative Context

- **Pattern**: Customer-Supplier
- **Direction**: Billing (downstream) depends on Patient Context (upstream) for insurance information and patient identity
- **Integration**: Patient Context provides insurance details; Billing maintains its own AccountHolder representation
- **Translation**: Insurance policy details are translated into billing-specific coverage structures

### Clinical/EMR Context ↔ Billing/Administrative Context

- **Pattern**: Customer-Supplier with Anti-Corruption Layer
- **Direction**: Billing (downstream) depends on Clinical (upstream) for encounter data and diagnosis codes
- **Integration**: Clinical context publishes EncounterCompleted events with diagnoses and procedures; Billing translates these into billable charges using CPT and ICD-10 coding
- **Translation**: Anti-corruption layer converts clinical terminology into billing codes

### Patient Context ↔ Emergency Services Context

- **Pattern**: Shared Kernel
- **Direction**: Bidirectional — Emergency needs patient identity; Patient context needs to know about ED visits
- **Integration**: Minimal shared model for patient identification (MRN, name, date of birth, allergies)
- **Rationale**: Emergency care requires immediate access to critical patient data without delays

### Clinical/EMR Context ↔ Emergency Services Context

- **Pattern**: Partnership
- **Direction**: Bidirectional — Emergency encounters transition to clinical encounters upon admission
- **Integration**: Emergency context publishes PatientAdmittedFromED; Clinical context creates an inpatient encounter
- **Coordination**: Teams coordinate on shared workflows like trauma activation

### Appointment/Scheduling Context ↔ Clinical/EMR Context

- **Pattern**: Customer-Supplier
- **Direction**: Clinical (downstream) consumes schedule information for encounter creation
- **Integration**: Scheduling publishes AppointmentCheckedIn; Clinical creates an outpatient encounter

### External Systems

- **Insurance APIs** → Billing Context: Anti-Corruption Layer translates proprietary insurance formats
- **Lab Systems** → Clinical/EMR Context: Open Host Service with Published Language (HL7/FHIR)
- **Pharmacy Systems** → Clinical/EMR Context: Conformist pattern using pharmacy system's data model
- **Government Reporting** → Billing Context: Anti-Corruption Layer formats data per regulatory requirements

## Integration Patterns Summary

| Upstream | Downstream | Pattern | Mechanism |
|----------|-----------|---------|-----------|
| Patient | Clinical/EMR | Customer-Supplier | Domain Events |
| Patient | Billing | Customer-Supplier | Domain Events |
| Clinical/EMR | Billing | Customer-Supplier + ACL | Domain Events |
| Patient | Emergency | Shared Kernel | Shared Data |
| Clinical/EMR | Emergency | Partnership | Domain Events |
| Scheduling | Clinical/EMR | Customer-Supplier | Domain Events |
| External Labs | Clinical/EMR | Open Host Service | HL7/FHIR |
| Insurance APIs | Billing | ACL | REST/EDI |

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — The nodes on the context map
- [Strategic Design](../strategic-design/) — Context mapping is a strategic design activity
- [Anti-Corruption Layer](../anti-corruption-layer/) — Key pattern used in multiple relationships
- [Integration Patterns](../integration-patterns/) — Detailed description of each pattern
- [Domain Events](../domain-events/) — Primary mechanism for inter-context communication

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
