# Integration Patterns in the Psychiatry Domain

## Overview

Integration patterns define how bounded contexts communicate and share information. In Domain-Driven Design, several standard integration patterns describe the relationship dynamics between contexts. The psychiatry domain uses multiple integration patterns, selected based on the clinical workflow requirements, data sharing needs, and organizational dynamics between the teams responsible for each bounded context.

## Patterns Used in the Psychiatry Domain

### Customer-Supplier

In this pattern, one context (the supplier or upstream) provides data that another context (the customer or downstream) depends on. The supplier accommodates the customer's needs but maintains authority over its own model.

**Diagnostic Assessment (Supplier) to Medication Management (Customer)**

The Diagnostic Assessment Context supplies diagnostic information that the Medication Management Context needs to determine prescribing indications. The diagnostic context publishes Diagnosis Assigned events in a format agreed upon with the medication context team. The diagnostic team accepts responsibility for notifying downstream consumers of model changes that affect published events.

Negotiations: The medication team requests that diagnostic events include severity specifiers because severity affects medication selection. The diagnostic team agrees to include specifiers in the Diagnosis Assigned event payload.

**Diagnostic Assessment (Supplier) to Psychotherapy (Customer)**

The Diagnostic Assessment Context supplies diagnoses that the Psychotherapy Context uses for modality and protocol selection. The psychotherapy team requests that diagnostic events include comorbidity flags because comorbid diagnoses affect treatment protocol selection (e.g., comorbid substance use modifies CBT protocol selection for depression). The diagnostic team includes comorbidity indicators in the published event.

### Shared Kernel

In this pattern, two or more contexts share a small, explicitly defined subset of the domain model. Changes to the shared model require agreement from all participating contexts.

**Crisis Management, Diagnostic Assessment, and Medication Management**

These three contexts share a narrow kernel consisting of:
- Patient identifier and demographics
- Active diagnosis list (code, specifiers, assignment date)
- Current medication list (medication name, dosage, start date)

This shared kernel exists because crisis situations require immediate access to diagnostic and medication information without the latency of asynchronous event processing. The kernel is deliberately narrow to minimize coupling.

Governance: Changes to the shared kernel data structures require approval from the clinical leads of all three contexts. The shared kernel is versioned independently and has its own change management process.

### Conformist

In this pattern, the downstream context accepts the upstream context's model without negotiation. The downstream context conforms to whatever the upstream provides.

**All Contexts (Upstream) to Outcomes Measurement (Conformist)**

The Outcomes Measurement Context acts as a conformist consumer of events from all other contexts. It accepts diagnostic events, medication events, therapy events, crisis events, and rehabilitation events in whatever format the producing contexts publish. The outcomes team does not impose requirements on upstream contexts because measurement should adapt to clinical data, not constrain clinical workflows.

This pattern is appropriate because:
- Outcomes measurement must accommodate diverse clinical data types.
- Imposing measurement requirements on clinical contexts would compromise care delivery.
- The outcomes team has the flexibility to transform received data into measurement-friendly formats internally.

### Partnership

In this pattern, two contexts have a mutual dependency and coordinate their development collaboratively. Neither context dominates; they negotiate integration points as peers.

**Psychotherapy and Rehabilitation Services**

These two contexts serve overlapping patient populations and coordinate on treatment goals. Patients in psychosocial rehabilitation often receive concurrent psychotherapy, and functional improvement goals span both contexts. The teams meet regularly to align:
- Treatment goal terminology and measurement approaches
- Session scheduling to avoid conflicts
- Progress reporting formats for shared patients
- Referral workflows between contexts

Neither context conforms to the other; they evolve their integration points collaboratively.

### Anti-Corruption Layer

Used at external system boundaries to translate between the psychiatry domain model and external system representations. See the dedicated Anti-Corruption Layer topic for detailed coverage.

Key integrations:
- EHR systems (HL7 FHIR translation)
- Pharmacy systems (NCPDP SCRIPT translation)
- Insurance billing (837P claim format translation)
- State reporting systems (jurisdiction-specific format translation)

### Published Language

In this pattern, a context defines a well-documented interchange format that external consumers can use without direct coordination.

**Outcomes Measurement Published Language**

The Outcomes Measurement Context publishes quality reports in a standardized format that external systems (accreditation bodies, state health departments, payer organizations) can consume. This published language follows HEDIS measure specifications and CMS quality reporting formats, providing a stable, documented interface that does not require bilateral negotiation with each consumer.

### Open Host Service

In this pattern, a context provides a well-defined protocol (service interface) for other contexts or external systems to consume.

**Diagnostic Assessment Open Host Service**

The Diagnostic Assessment Context provides a read-only service interface that authorized consumers can use to query diagnostic information. This is used by the Crisis Management shared kernel and by clinical decision support tools. The interface is documented, versioned, and maintained as a stable contract.

## Integration Pattern Selection Criteria

The selection of integration pattern for each context relationship is guided by:

- **Clinical urgency**: High-urgency integrations (crisis management) use shared kernel or synchronous access; routine integrations use event-driven customer-supplier.
- **Power dynamics**: When one context clearly depends on another, customer-supplier is used. When contexts are peers, partnership is used.
- **Flexibility requirements**: When a downstream context must accept whatever is provided, conformist is used.
- **External system boundaries**: Anti-corruption layers are always used at external system boundaries.
- **Stability requirements**: When external consumers need stable interfaces, published language or open host service is used.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context map patterns.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping patterns.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
