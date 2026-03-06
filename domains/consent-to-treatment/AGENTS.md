# Consent to Treatment Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to consent-to-treatment processes in healthcare, encompassing informed consent documentation and workflows, patient capacity assessment, legal and regulatory consent requirements, consent revocation and withdrawal, proxy and surrogate decision-making, and clinical procedure authorization.

## Bounded Contexts

1. Informed Consent - Manages the disclosure of information to patients about proposed treatments, including risks, benefits, alternatives, and the patient's voluntary agreement or refusal.
2. Capacity Assessment - Handles the evaluation of whether a patient has the mental capacity to understand, retain, weigh, and communicate a treatment decision at the time consent is sought.
3. Proxy Decision-Making - Governs situations where a patient lacks capacity and a legally authorized representative, such as a healthcare proxy, power of attorney, or court-appointed guardian, makes decisions on the patient's behalf.
4. Consent Lifecycle - Tracks the full lifecycle of a consent record from initial creation through verification, renewal, modification, withdrawal, and archival, ensuring traceability and audit compliance.

## Domain Principles

- Model using shared ubiquitous language between clinicians, patients, legal advisors, ethics committees, and healthcare administrators.
- Group the system into bounded contexts reflecting the distinct workflows of information disclosure, capacity evaluation, proxy authorization, and consent record management.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow the Mental Capacity Act 2005 (England and Wales), the common law doctrine of informed consent, the General Medical Council (GMC) guidelines, and applicable international frameworks such as the Declaration of Helsinki.

## Key Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- informed-consent
- capacity-assessment
- proxy-decision-making
- consent-lifecycle
- event-driven-architecture
- integration-patterns
- consent-to-treatment-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Beauchamp, Tom L., and James F. Childress. Principles of Biomedical Ethics. 8th ed. Oxford University Press, 2019.
- General Medical Council. Decision Making and Consent. GMC, 2020.
- Mental Capacity Act 2005, Code of Practice. UK Department for Constitutional Affairs, TSO, 2007.
