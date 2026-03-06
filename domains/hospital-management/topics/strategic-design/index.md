# Strategic Design in Hospital Management

## Overview

Strategic design in Domain-Driven Design (DDD) addresses how to decompose a large, complex hospital system into manageable parts that align with the organizational structure and clinical workflows. It focuses on identifying the most important areas of the domain, defining clear boundaries between subsystems, and establishing how those subsystems communicate.

## Relevance to Hospital Management

Hospital systems are among the most complex enterprise domains, involving clinical care, administration, finance, regulatory compliance, and logistics. Strategic design provides the tools to manage this complexity by:

- Aligning software models with how clinicians, administrators, and staff actually think about their work
- Separating concerns so that changes in billing do not ripple into clinical care
- Identifying which parts of the system deserve the most investment and the best engineering talent
- Enabling independent teams to work on different bounded contexts without constant coordination

## Key Concepts

### Knowledge Crunching

Knowledge crunching is the collaborative process of extracting domain knowledge from clinical experts and translating it into a software model. In hospital management, this means:

- Conducting workshops with physicians, nurses, pharmacists, and administrators
- Observing clinical workflows such as patient admission, triage, and discharge
- Iteratively refining the model as understanding deepens
- Resolving ambiguities in terminology (e.g., "order" means different things in clinical vs. billing contexts)

### Domain Vision Statement

A domain vision statement captures the core purpose and value of the system. For a hospital management system, this might be:

> "To provide a unified platform that supports safe, efficient patient care by modeling clinical workflows, administrative processes, and regulatory requirements as first-class domain concepts, enabling clinicians to focus on patient outcomes rather than system complexity."

### Core Domain Identification

Not all parts of the hospital system are equally important. Strategic design distinguishes:

- **Core Domain**: Clinical care coordination, patient safety, and treatment management — the areas that differentiate the hospital and directly impact patient outcomes
- **Supporting Subdomains**: Scheduling, inventory management, staff rostering — necessary but not the primary differentiator
- **Generic Subdomains**: Authentication, email notifications, logging — commoditized capabilities available as off-the-shelf solutions

### Distillation

Distillation separates the essential complexity of the core domain from the accidental complexity of supporting and generic concerns. In hospital management:

- Clinical decision support logic is core and deserves custom modeling
- Insurance claim formatting is supporting and may use established patterns
- User authentication is generic and should use standard libraries

## Hospital Domain Decomposition

Strategic design guides the hospital system into bounded contexts:

- **Patient Context** — Registration, demographics, medical history, identity management
- **Appointment/Scheduling Context** — Provider schedules, resource allocation, appointment lifecycle
- **Clinical/EMR Context** — Encounters, orders, results, treatment plans, clinical notes
- **Billing/Administrative Context** — Coding, claims, payments, financial reporting
- **Emergency Services Context** — Triage, trauma protocols, capacity management

Each context has its own model, language, and team ownership, reducing cognitive load and enabling independent evolution.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — The primary output of strategic design
- [Context Map](../context-map/) — Visualizes relationships between bounded contexts
- [Subdomains](../subdomains/) — Classification of domain areas by strategic importance
- [Ubiquitous Language](../ubiquitous-language/) — Shared vocabulary within each bounded context
- [Anti-Corruption Layer](../anti-corruption-layer/) — Protects context boundaries from external system leakage

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapters 14–16. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Part I: Strategic Design. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Part III. Wrox, 2015.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
