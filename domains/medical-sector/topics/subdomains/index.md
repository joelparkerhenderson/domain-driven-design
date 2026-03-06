# Subdomains in Medical Practice

## Overview

Subdomain classification is the process of categorizing areas of a domain by their strategic importance to the organization. In Domain-Driven Design, subdomains are classified as core, supporting, or generic. In medical practice, this classification guides investment decisions, team allocation, and build-versus-buy choices across clinical care, diagnostics, pharmacy, insurance, and telemedicine.

## Subdomain Types

### Core Subdomains

Core subdomains are the areas where the medical practice system creates the most value and competitive differentiation. They require the deepest domain expertise, the most talented engineers, and custom-built solutions.

In the medical domain, core subdomains include:

- **Clinical Decision Support** -- Evidence-based rules, alerts, and recommendations that improve patient safety and care quality at the point of care
- **Medication Safety** -- Drug interaction checking, allergy cross-referencing, dosage validation, and controlled substance monitoring
- **Care Coordination** -- Orchestrating care plans, referrals, and transitions across providers, specialties, and settings
- **Patient Record Integrity** -- Ensuring the accuracy, completeness, and longitudinal consistency of patient health information

### Supporting Subdomains

Supporting subdomains are necessary for the system to function but are not the primary differentiator. They may use established patterns and frameworks rather than fully custom solutions.

In the medical domain, supporting subdomains include:

- **Insurance Claims Processing** -- Claims generation, submission, and tracking following established EDI standards
- **Referral Management** -- Routing referral requests between providers and tracking their status
- **Telehealth Scheduling** -- Managing virtual visit appointments and provider availability
- **Lab Order Routing** -- Directing lab orders to appropriate internal or external laboratories
- **Formulary Management** -- Maintaining lists of covered medications and their tier assignments

### Generic Subdomains

Generic subdomains are commoditized capabilities that can be purchased off-the-shelf or implemented using standard libraries. They do not warrant custom domain modeling.

In the medical domain, generic subdomains include:

- **User Authentication and Authorization** -- Identity management, single sign-on, role-based access control
- **Notification Services** -- Email, SMS, and push notification delivery
- **Audit Logging** -- Recording system access and changes for compliance (the logging mechanism itself, not the compliance rules)
- **Document Storage** -- File storage for scanned documents, images, and attachments
- **Calendar and Scheduling Infrastructure** -- Basic time-slot management and calendar synchronization

## Strategic Investment Guidance

| Subdomain Type | Investment Level | Build vs. Buy | Team Quality |
|---------------|-----------------|---------------|-------------|
| Core | Highest | Always build custom | Best engineers, deep domain expertise |
| Supporting | Moderate | Build with frameworks, consider SaaS | Competent engineers |
| Generic | Minimal | Buy off-the-shelf, use libraries | Standard implementation |

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Subdomain classification is a key strategic design activity
- [Bounded Contexts](../bounded-contexts/) -- Subdomains often align with bounded contexts, though the mapping is not always one-to-one
- [Context Map](../context-map/) -- Shows how subdomains interact and depend on each other
- [Domain Services](../domain-services/) -- Core subdomain logic often lives in domain services
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects core subdomains from generic and external system complexity

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 15. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 2. Addison-Wesley, 2016.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 3. O'Reilly, 2021.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 3. Wrox, 2015.
