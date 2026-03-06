# Subdomains in Hospital Management

## Overview

Subdomains are the natural divisions of a business domain based on organizational capabilities and strategic importance. Domain-Driven Design classifies subdomains into three categories — core, supporting, and generic — to guide where to invest the most design effort and engineering talent.

## Relevance to Hospital Management

Hospital systems span dozens of functional areas. Not all areas are equally important to the hospital's mission of patient care. Subdomain classification helps:

- Focus the best developers and deepest modeling on clinical care (core domain)
- Apply pragmatic solutions to scheduling and billing (supporting subdomains)
- Use off-the-shelf products for authentication and email (generic subdomains)
- Allocate budget proportionally to strategic importance

## Subdomain Classification

### Core Domain: Clinical Care and Patient Management

The core domain is what makes the hospital system uniquely valuable. It directly impacts patient outcomes and competitive differentiation.

- **Clinical Care Coordination**: Encounter management, clinical decision support, order management, treatment planning
- **Patient Safety**: Allergy checking, drug interaction alerts, clinical alerts and escalations
- **Emergency Medicine**: Triage algorithms, trauma protocols, surge capacity management
- **Care Transitions**: Admission, transfer, discharge workflows with safety checks

**Investment strategy**: Custom-built, deeply modeled, staffed with senior engineers and close collaboration with clinicians.

### Supporting Subdomains

Supporting subdomains are necessary for the hospital to function but are not the primary differentiator. They support the core domain.

- **Appointment Scheduling**: Provider availability, booking, waitlists, recurring visits
- **Billing and Revenue Cycle**: Charge capture, claims submission, payment processing, denial management
- **Inventory and Supply Chain**: Medical supplies, pharmaceuticals, equipment tracking
- **Staff Management**: Nurse assignments, physician credentialing, shift scheduling
- **Bed Management**: Bed availability, housekeeping status, patient placement

**Investment strategy**: Well-modeled but may follow established industry patterns. Less need for novel design.

### Generic Subdomains

Generic subdomains provide commoditized capabilities that are not specific to healthcare. Off-the-shelf solutions are preferred.

- **Authentication and Authorization**: User login, role-based access control, single sign-on
- **Email and Notifications**: Appointment reminders, alert notifications, system messages
- **Logging and Auditing**: System logs, access logs (though HIPAA audit trails are part of the core)
- **Document Management**: File storage, document versioning, attachments
- **Reporting Infrastructure**: Report generation, data export, dashboard frameworks

**Investment strategy**: Buy or adopt open-source. Do not build custom. Integrate via anti-corruption layers.

## Identifying Subdomains

### Techniques

- **Organizational structure**: Departments often align with subdomains (Emergency, Radiology, Finance)
- **Expert interviews**: Ask "What is the most important thing your department does?"
- **Competitive analysis**: What differentiates this hospital from others?
- **Cost of failure**: Where does a software failure cause the most harm? (Core domain)
- **Replaceability test**: Could this capability be replaced by a third-party product without affecting the hospital's mission? (Generic subdomain)

### Hospital Subdomain Map

| Subdomain | Classification | Bounded Context | Priority |
|-----------|---------------|----------------|----------|
| Clinical Care | Core | Clinical/EMR | Highest |
| Patient Identity | Core | Patient | Highest |
| Emergency Medicine | Core | Emergency Services | Highest |
| Scheduling | Supporting | Appointment/Scheduling | Medium |
| Billing | Supporting | Billing/Administrative | Medium |
| Inventory | Supporting | (Future context) | Medium |
| Authentication | Generic | (External system) | Low |
| Email/Notifications | Generic | (External system) | Low |

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Subdomain identification is a strategic design activity
- [Bounded Contexts](../bounded-contexts/) — Each subdomain typically maps to one or more bounded contexts
- [Context Map](../context-map/) — Shows relationships between subdomain-aligned contexts
- [Anti-Corruption Layer](../anti-corruption-layer/) — Protects core domain from generic subdomain implementations

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 15: Distillation. Addison-Wesley, 2003.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 2: Strategic Design with Bounded Contexts and the Ubiquitous Language. Addison-Wesley, 2016.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 3: Focusing on the Core Domain. Wrox, 2015.
