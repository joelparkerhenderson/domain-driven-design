# Advance Decision to Refuse Treatment Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to advance decisions to refuse treatment (also known as advance directives or living wills), encompassing patient autonomy and decision-making capacity, legal validity and witnessing requirements, treatment specification and refusal scope, clinical applicability and emergency protocols, stakeholder notification and record-keeping, and revocation and amendment processes.

## Bounded Contexts

1. Decision Authoring - Captures the patient's wishes regarding specific treatments they wish to refuse, including the circumstances under which the refusal applies and the documentation of the decision.
2. Capacity Assessment - Manages the evaluation of whether a person has the mental capacity to make an advance decision, including assessments by qualified professionals and the documentation of capacity findings.
3. Legal Validation - Ensures that advance decisions meet statutory requirements for validity and applicability, including witnessing, signatures, and compliance with the Mental Capacity Act 2005 or equivalent legislation.
4. Clinical Application - Governs how healthcare professionals identify, interpret, and apply advance decisions during treatment episodes, particularly in emergency and end-of-life scenarios.
5. Notification and Distribution - Manages the communication of advance decisions to relevant parties including healthcare providers, family members, legal representatives, and care institutions.
6. Revocation and Amendment - Handles the processes by which a person can withdraw, modify, or update their advance decision while they retain capacity to do so.

## Domain Principles

- Model using shared ubiquitous language between patients, clinicians, legal professionals, social workers, and family advocates.
- Group the system into bounded contexts reflecting healthcare, legal, and patient-autonomy workflows.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow Mental Capacity Act 2005 (UK), Patient Self-Determination Act (US), and equivalent jurisdictional legislation.
- Maintain patient consent, data protection (GDPR/HIPAA), and clinical safety as cross-cutting concerns.

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
- decision-authoring
- capacity-assessment
- legal-validation
- clinical-application
- notification-and-distribution
- revocation-and-amendment
- event-driven-architecture
- integration-patterns
- advance-decision-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Department for Constitutional Affairs. Mental Capacity Act 2005: Code of Practice. The Stationery Office, 2007.
- Sabatino, Charles P. "The Evolution of Health Care Advance Planning Law and Policy." The Milbank Quarterly, vol. 88, no. 2, 2010, pp. 211-239.
