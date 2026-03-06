# Bounded Contexts in Hospital Management

## Overview

A bounded context is a well-defined boundary within which a particular domain model applies and terms have specific, unambiguous meaning. In hospital management, bounded contexts prevent the confusion that arises when the same term (e.g., "patient," "order," "encounter") means different things in different parts of the organization.

## Relevance to Hospital Management

Hospitals are inherently multi-disciplinary. A patient means something different to a registration clerk, a nurse, a billing specialist, and an emergency physician. Without bounded contexts, a single "Patient" model would accumulate every attribute and behavior for every use case, becoming unmaintainable. Bounded contexts let each team own a focused model that serves their specific needs.

## The Five Hospital Bounded Contexts

### Patient Context

- **Focus**: Patient identity, demographics, registration, medical history, insurance information, emergency contacts, allergies
- **Aggregate Root**: Patient (identified by Medical Record Number)
- **Key Operations**: Register, update demographics, merge duplicate records, manage consent
- **Language**: Patient, MRN, Next of Kin, Insurance Policy, Allergy, Medical History
- **Team**: Patient Access / Registration

### Appointment/Scheduling Context

- **Focus**: Provider schedules, appointment booking, resource allocation, waitlists, recurring visits
- **Aggregate Root**: Appointment
- **Key Operations**: Schedule, reschedule, cancel, check conflicts, manage waitlist
- **Language**: Appointment, TimeSlot, Schedule, Provider Availability, Waitlist, Overbooking
- **Team**: Scheduling / Operations

### Clinical/EMR Context

- **Focus**: Clinical encounters, orders (lab, imaging, pharmacy), results, clinical notes, treatment plans, diagnoses
- **Aggregate Root**: Encounter
- **Key Operations**: Start encounter, place orders, record results, write clinical notes, close encounter
- **Language**: Encounter, Order, Lab Result, Prescription, Diagnosis, Clinical Note, Treatment Plan
- **Team**: Clinical Informatics / IT

### Billing/Administrative Context

- **Focus**: Charge capture, medical coding (CPT, ICD-10), insurance claims, payments, invoices, financial reporting
- **Aggregate Root**: Claim
- **Key Operations**: Generate charges, submit claims, process payments, handle denials, produce financial reports
- **Language**: Charge, Claim, CPT Code, ICD-10 Code, Remittance, Denial, Adjustment
- **Team**: Revenue Cycle / Finance

### Emergency Services Context

- **Focus**: Triage, trauma activation, rapid assessment, bed management, capacity tracking, mass casualty protocols
- **Aggregate Root**: EmergencyEncounter
- **Key Operations**: Triage, assign ESI level, activate trauma team, track bed availability, manage surge capacity
- **Language**: Triage, ESI Level, Trauma Activation, Resuscitation Bay, Fast Track, Surge Protocol
- **Team**: Emergency Department

## Boundary Enforcement

Each bounded context enforces its boundary by:

- Maintaining its own data store or schema
- Exposing only well-defined interfaces (APIs, events) to other contexts
- Translating external concepts through anti-corruption layers
- Owning its deployment lifecycle independently

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Bounded contexts are the primary output of strategic design
- [Context Map](../context-map/) — Shows how bounded contexts relate and communicate
- [Ubiquitous Language](../ubiquitous-language/) — Each bounded context defines its own language
- [Anti-Corruption Layer](../anti-corruption-layer/) — Protects context boundaries during integration
- [Integration Patterns](../integration-patterns/) — Defines how contexts exchange information

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 6. Wrox, 2015.
