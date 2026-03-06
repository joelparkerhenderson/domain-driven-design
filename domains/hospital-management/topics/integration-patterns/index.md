# Integration Patterns in Hospital Management

## Overview

Integration patterns define how bounded contexts communicate and share data. Domain-Driven Design identifies several patterns for managing relationships between contexts, each with different trade-offs in coupling, autonomy, and complexity. In hospital management, these patterns govern how patient data flows from registration to clinical care to billing.

## Relevance to Hospital Management

Hospital systems must integrate internally (between bounded contexts) and externally (with labs, pharmacies, insurance companies, government agencies). The choice of integration pattern for each relationship determines:

- How tightly coupled the contexts are
- How much autonomy each team has to evolve its model
- How data inconsistencies are handled
- How resilient the system is to failures in one context

## DDD Integration Patterns

### Shared Kernel

Two or more bounded contexts share a small, explicitly defined subset of the domain model. Both teams must agree on changes to the shared code.

**Hospital example**: Patient Context and Emergency Context share a minimal patient identity model (MRN, name, date of birth, allergies, blood type). This enables the ED to access life-critical data without event propagation delays.

- **Strength**: Immediate data access, no translation needed
- **Weakness**: Changes to the shared model require coordination between teams
- **When to use**: When two contexts need real-time access to the same critical data and the shared surface is small

### Customer-Supplier

The upstream context (supplier) provides data or services that the downstream context (customer) depends on. The upstream team considers the downstream team's needs when planning changes.

**Hospital examples**:

- Patient (supplier) → Clinical/EMR (customer): Clinical context depends on patient demographics
- Patient (supplier) → Billing (customer): Billing depends on insurance information
- Clinical/EMR (supplier) → Billing (customer): Billing depends on encounter data and diagnosis codes
- Scheduling (supplier) → Clinical/EMR (customer): Clinical depends on appointment check-in events

- **Strength**: Clear dependency direction, upstream can evolve with downstream input
- **Weakness**: Downstream is dependent on upstream's release schedule and data quality

### Published Language

A well-documented, shared data format used for communication between contexts. Neither context's internal model is exposed; instead, both translate to/from the published language.

**Hospital example**: HL7 FHIR resources serve as a published language for clinical data exchange. The Clinical context translates internal Encounter aggregates to FHIR Encounter resources when communicating with external systems. The Billing context translates FHIR ExplanationOfBenefit resources into internal Claim objects.

- **Strength**: Stable interface, widely understood, enables integration with external systems
- **Weakness**: Requires translation on both sides; published language may not perfectly fit either context

### Open Host Service

A bounded context exposes a well-defined API (the "open host") that any consumer can use. Combined with Published Language, it provides a standard integration point.

**Hospital example**: The Clinical/EMR Context exposes a FHIR-compliant API that external lab systems, pharmacy systems, and referring hospitals can use to submit results, query patient data, or exchange clinical documents.

- **Strength**: Standard interface, multiple consumers without custom integrations
- **Weakness**: API must accommodate diverse consumer needs; versioning is critical

### Anti-Corruption Layer

A translation layer that protects a bounded context's model from external system concepts. The ACL adapts external data into internal domain objects.

**Hospital examples**:

- Insurance APIs → Billing Context: EDI formats translated to domain Claim objects
- Lab systems → Clinical/EMR: HL7 v2 messages translated to domain LabResult entities
- Government reporting → Billing: Internal data formatted into CMS-required formats

- **Strength**: Full isolation from external model changes
- **Weakness**: Additional complexity and maintenance of the translation layer

### Conformist

The downstream context adopts the upstream context's model without translation. The downstream team gives up model autonomy to avoid the cost of building a translation layer.

**Hospital example**: Pharmacy integration where the hospital system adopts the pharmacy system's medication model (NDC codes, dispensing units) directly rather than translating to internal representations.

- **Strength**: Zero translation cost, faster integration
- **Weakness**: Downstream model is constrained by upstream's design decisions

### Separate Ways

Two contexts have no integration at all. They operate independently with no data exchange.

**Hospital example**: The hospital's research data warehouse operates independently from the clinical system. Data is extracted periodically via ETL rather than real-time integration.

- **Strength**: Complete autonomy, no coupling
- **Weakness**: Data duplication, potential inconsistency

## Hospital Integration Map

| Upstream | Downstream | Pattern | Mechanism |
|----------|-----------|---------|-----------|
| Patient | Clinical/EMR | Customer-Supplier | Domain events |
| Patient | Billing | Customer-Supplier | Domain events |
| Patient | Emergency | Shared Kernel | Shared data model |
| Clinical/EMR | Billing | Customer-Supplier + ACL | Domain events with translation |
| Clinical/EMR | Emergency | Partnership | Domain events, shared workflows |
| Scheduling | Clinical/EMR | Customer-Supplier | Domain events |
| External Labs | Clinical/EMR | Open Host + Published Language | HL7 FHIR API |
| Insurance APIs | Billing | ACL | EDI 837/835 |
| Pharmacy Systems | Clinical/EMR | Conformist | NCPDP SCRIPT |

## Relationships to Other Topics

- [Context Map](../context-map/) — Integration patterns are documented on the context map
- [Anti-Corruption Layer](../anti-corruption-layer/) — Detailed documentation of the ACL pattern
- [Bounded Contexts](../bounded-contexts/) — Integration patterns connect bounded contexts
- [Domain Events](../domain-events/) — Events are the primary mechanism for asynchronous integration
- [Event-Driven Architecture](../event-driven-architecture/) — The infrastructure that supports event-based integration
- [Healthcare Standards](../healthcare-standards/) — HL7, FHIR serve as published languages

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 7: Context Mapping. Wrox, 2015.
